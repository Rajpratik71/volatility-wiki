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
| quicksqlite | Faster version of sqlite (uses executemany) | 
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

## Using the sqlite/quicksqlite renderers

Here's an example of using the sqlite (or quicksqlite) renderer: 

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

# Plugin Consumers