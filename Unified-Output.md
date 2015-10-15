The unified output in Volatility (available since 2.5) aims to give users the flexibility of asking for their output in a specific format (text, json, sqlite, html, etc) while simplifying things for developers. In particular, the "body" of a plugin can be written once and its return values can be re-used by multiple renderers. In short (no puns intended) less code leads to more functionality. Also on the developer side, since plugins can output json or sqlite, it makes building frameworks, GUIs, and libraries on top of Volatility much easier. 

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

# Plugin Developers

# Plugin Consumers