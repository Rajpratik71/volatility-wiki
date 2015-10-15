The unified output in Volatility (available since 2.5) aims to give users the flexibility of asking for their output in a specific format (text, json, sqlite, html, etc) while simplifying things for developers. In particular, the "body" of a plugin can be written once and its return values can be re-used by multiple renderers. In short (no puns intended) less code leads to more functionality. Also on the developer side, since plugins can output json or sqlite, it makes building frameworks, GUIs, and libraries on top of Volatility much easier. 

# Standard Renderers 

Here's a summary of the existing standard renderers:

| Name | Description | 
|------|--------------|
| text | The "normal" text based table output |
| dot | Graphviz dot diagrams | 
| html | Uses HTML to render data | 
| json | Outputs results as JSON - useful for APIs | 
| sqlite | Populates an sqlite3 database |
| quick | Faster version of text, but pipe separated values | 
| xlsx | Outputs results as an Excel spreadsheet (requires openpyxl) |

# Command Line Users

A plugin's supported formats are displayed in the `-h` output:

```
$ python vol.py pslist -h
Volatility Foundation Volatility Framework 2.5
Usage: Volatility - A memory forensics analysis platform.

[snip]

Module Output Options: dot, html, json, quick, quicksqlite, sqlite, text, xlsx

---------------------------------
Module PSList
---------------------------------
Print all running processes by following the EPROCESS lists
```

## Using the dot renderer 

Here's an example of using the dot renderer: 

```
$ python vol.py -f grrcon.img pslist --output=dot --output-file=pslist.dot 
Volatility Foundation Volatility Framework 2.5
```

When you open the resulting file in a Graphviz viewer, it should appear like this:

[[images/pslist_dot.png]]

## Using the html renderer 

Here's an example of using the html renderer: 

```
$ python vol.py -f grrcon.img pslist --output=html --output-file=pslist.html 
Volatility Foundation Volatility Framework 2.5
```

When you open the resulting file in a browser, you should be able to sort columns, search text fields, etc. 

[[images/pslist_html.png]]

## Using the xlsx renderer 

Here's an example of using the xlsx renderer: 

```
$ python vol.py -f grrcon.img pslist --output=xlsx --output-file=pslist.xlsx 
Volatility Foundation Volatility Framework 2.5
```

When you open the resulting file in a spreadsheet viewer:

[[images/pslist_xlsx.png]]

## Using the json renderer 

Here's an example of using the json renderer: 

```
$ python vol.py -f grrcon.img pslist --output=json --output-file=pslist.json 
Volatility Foundation Volatility Framework 2.5
```

You can now consume the pslist.json file however you like. Note that the json contains two dictionary keys: columns and rows. Columns tells you the column header names and there's one row per result (with values ordered according to the column header positions). 

```
$ python -m json.tool < pslist.json 
{
    "columns": [
        "Offset(V)",
        "Name",
        "PID",
        "PPID",
        "Thds",
        "Hnds",
        "Sess",
        "Wow64",
        "Start",
        "Exit"
    ],
    "rows": [
        [
            2185005104,
            "System",
            4,
            0,
            51,
            269,
            -1,
            0,
            "",
            ""
        ],
        [
            2182193184,
            "smss.exe",
            360,
            4,
            3,
            19,
            -1,
            0,
            "2012-04-28 01:56:37 UTC+0000",
            ""
        ],
[snip]
```

## Using the sqlite renderer

Here's an example of using the sqlite renderer: 

```
$ python vol.py -f grrcon.img pslist --output=sqlite --output-file=pslist.db 
Volatility Foundation Volatility Framework 2.5
```

You can then explore and query the database:

```
$ sqlite3 pslist.db 
SQLite version 3.7.13 2012-07-17 17:46:21
Enter ".help" for instructions
Enter SQL statements terminated with a ";"
sqlite> .schema
CREATE TABLE PSList( id INTEGER, rowparent INTEGER, "Offset(V)" TEXT, "Name" TEXT, "PID" TEXT, "PPID" TEXT, "Thds" TEXT, "Hnds" TEXT, "Sess" TEXT, "Wow64" TEXT, "Start" TEXT, "Exit" TEXT);
sqlite> select * from PSList limit 5;
1|0|2185005104|System|4|0|51|269|-1|0||
2|0|2182193184|smss.exe|360|4|3|19|-1|0|2012-04-28 01:56:37 UTC+0000|
3|0|2182255136|csrss.exe|596|360|11|340|0|0|2012-04-28 01:56:38 UTC+0000|
4|0|2182692896|winlogon.exe|624|360|17|535|0|0|2012-04-28 01:56:39 UTC+0000|
5|0|2182374496|services.exe|672|624|15|238|0|0|2012-04-28 01:56:39 UTC+0000|
sqlite> .quit
```

# Plugin Developers

The plugin template below identifies the necessary components for building a plugin with unified output. The key is defining the data you want to display in your plugin (i.e. the column headers) and their data types (strings, integers, addresses, etc). 

The sample plugin also shows how you would override a default renderer and how to register a new one. 

```
import volatility.plugins.common as common 
import volatility.utils as utils 

from volatility.renderers import TreeGrid
from volatility.renderers.basic import Address, Hex

class MyPlugin(common.AbstractWindowsCommand):
    """My test plugin"""

    def calculate(self):
        """Body of the plugin goes here"""

        addr_space = utils.load_as(self._config)
    
        ## gather some data from the memory dump using the address space 
        ## then yield each result to the generator/render functions

        data = [
            [0xFFFFFFFF80004210, "testing123", 400, 6500],
            [0xFFFFFFFF80008726, "testing345", 800, 124400],
            ]

        for result in data:
            yield result 

    def render_dot(self, outfd, data):
        """Here we can override the default dot renderer"""

        ...

    def render_test(self, outfd, data):
        """Here we register a new handler invoked with --output=test"""

        ...

    def unified_output(self, data):
        """This standardizes the output formatting"""
        
        ## make sure the number of columns (4) and their data types match
        ## what calculate() and generator() yields 

        return TreeGrid([("Offset", Address),
                        ("Name", str),
                        ("ID", int),
                        ("Count", Hex),
                        self.generator(data))

    def generator(self, data):
        """This yields data according to the unified output format"""

        ## the variables "unpacked" here must match what calculate() yields 
        for offset, name, id, count in data:

            ## make sure to wrap each variable according to its data type 
            yield (0, [Address(offset), str(name), int(id), Hex(count)])
```

# Framework Designers 

If you're designing a framework around Volatility that harvests/collects plugin output and then processes, morphs, and/or saves it according to your goals, we highly recommend using the JSON renderer as an API. This lets you directly access the results and do as you wish, rather than forking a Volatility process, getting the results as plain text, splitting the columns with regular expressions, and then converting fields back to numbers as needed, etc. 

See the two files in our [Library Example](https://github.com/volatilityfoundation/volatility/tree/master/contrib/library_example) for a demonstration of how this is possible. The [libapi.py](https://github.com/volatilityfoundation/volatility/blob/master/contrib/library_example/libapi.py) file exposes `get_config` and `get_json` APIs. You should not have to modify `libapi.py` at all for basic usage purposes. 

Also in the directory is [pslist_json.py](https://github.com/volatilityfoundation/volatility/blob/master/contrib/library_example/pslist_json.py) which shows how to leverage `libapi.py` for acquiring JSON output from the PSList plugin. It boils down to calling `get_config` to create the configuration from a given memory dump file location and Volatility profile:

```
config = libapi.get_config("/path/to/mem.dmp", "Win7SP1x64")
```

Then you would pass the config object and the desired plugin to `get_json` which returns the JSON data:

```
data = libapi.get_json(config, taskmods.PSList)
```

This `data` variable now contains what you see in the example above (using the json renderer) with the columns and rows dictionary keys. 
 