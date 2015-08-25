# Dependencies

In order to have complete support, you should install the following:

* [Python 2.7](https://www.python.org/download/releases/2.7/)
* [Pyinstaller](https://github.com/pyinstaller/pyinstaller)
* [Distorm](https://github.com/gdabah/distorm)
* [PyCrypto](https://www.dlitz.net/software/pycrypto/) ([Windows Binaries](http://www.voidspace.org.uk/python/modules.shtml#pycrypto))
* [Yara](http://plusvic.github.io/yara/)
* [OpenPyxl](https://bitbucket.org/openpyxl/openpyxl)
* [PIL](http://www.pythonware.com/products/pil/)

# Dependency Fixes

Distorm and OpenPyxl must be patched in order to allow Pyinstaller to find necessary files.  

## Installing and Fixing after `easy_install`

The easiest way to install Distorm3 and OpenPyxl are by using `easy_install`, which is bundled with [Setuptools](https://pypi.python.org/pypi/setuptools) or obtained by running the [`get-pip.py` script to install pip](https://bootstrap.pypa.io/get-pip.py).  After installation, the `easy_install` program is found at `C:\Python27\Scripts\easy_install.exe`.  Make sure this is either in your path, or that you invoke it directly:

```
> easy_install distorm3
[snip]
> easy_install openpyxl
[snip]
```

### Distorm3 Modification

Modify the `C:\Python27\Lib\site-packages\distorm3-3.3.0-py2.7-win32.egg\distorm3\__init__.py` file.  Around line 29, you'll see the following:

```
from ctypes import *
from os.path import split, join

#==============================================================================
# Load the diStorm DLL

# Guess the DLL filename and load the library.
_distorm_path = split(__file__)[0]
```

Modify this like so, new lines denoted by the unneeded comments "# this is new":

```
from ctypes import *
from os.path import split, join
import sys                         # this is new

#==============================================================================
# Load the diStorm DLL

# Guess the DLL filename and load the library.
_distorm_path = split(__file__)[0]
if hasattr(sys, '_MEIPASS'):       # this is new
    _distorm_path = sys._MEIPASS   # this is new
```

### OpenPyxl Modification

Modify the `C:\Python27\Lib\site-packages\openpyxl-2.3.0b1-py2.7.egg\openpyxl\__init__.py` file.  In the original file, you'll see the following:

```
import json
import os

here = os.path.abspath(os.path.dirname(__file__))
```

Add one import statement and two other lines of code around this, like so (unneeded comments "# this is new" used again):

```
import json
import os
import sys                       # this is new

here = os.path.abspath(os.path.dirname(__file__))
if hasattr(sys, '_MEIPASS'):     # this is new
    here = sys._MEIPASS          # this is new
```

## Installing/Fixing from Source

You may install these libraries from source instead, using their own `setup.py` scripts.  Below are patches to the `__init__.py` scripts for Distorm and OpenPyxl:

### Distorm3 Source Modification

```
diff --git a/python/distorm3/__init__.py b/python/distorm3/__init__.py
index 734c7c8..2345c1e 100644
--- a/python/distorm3/__init__.py
+++ b/python/distorm3/__init__.py
@@ -27,12 +27,15 @@ __all__ = [
 
 from ctypes import *
 from os.path import split, join
+import sys
 
 #==============================================================================
 # Load the diStorm DLL
 
 # Guess the DLL filename and load the library.
 _distorm_path = split(__file__)[0]
+if hasattr(sys, '_MEIPASS'):
+    _distorm_path = sys._MEIPASS
 potential_libs = ['distorm3.dll', 'libdistorm3.dll', 'libdistorm3.so', 'libdistorm3.dylib']
 lib_was_found = False
 for i in potential_libs:
```

*Note:* You may also clone the following repository, which has the appropriate changes: [https://github.com/gleeda/distorm](https://github.com/gleeda/distorm)

### OpenPyxl Source Modification

```
diff --git a/openpyxl/__init__.py b/openpyxl/__init__.py
index 464aeee..136f1bf 100644
--- a/openpyxl/__init__.py
+++ b/openpyxl/__init__.py
@@ -6,8 +6,11 @@
 
 import json
 import os
+import sys
 
 here = os.path.abspath(os.path.dirname(__file__))
+if hasattr(sys, '_MEIPASS'):
+    here = sys._MEIPASS
 src_file = os.path.join(here, ".constants.json")
 with open(src_file) as src:
     constants = json.load(src)
```

*Note:* You may also clone the following repository, which has the appropriate changes: [https://github.com/gleeda/openpyxl](https://github.com/gleeda/openpyxl)

# Compiling with Pyinstaller

After installing all of the dependencies and also [downloading the latest Volatility source code](https://github.com/volatilityfoundation/volatility/archive/master.zip), you are ready to create a Volatility binary.

## Windows

For Windows binaries, you may need a couple more dependencies:

* [Microsoft Visual C++ Compiler for Python 2.7 ](http://www.microsoft.com/en-us/download/details.aspx?id=44266)
* Visual Studio or at least the [VS C++ 2008 Redistributable Package (x86)](http://www.microsoft.com/en-us/download/confirmation.aspx?id=29)

Pyinstaller is located in `C:\Python27\Scripts` by default.  Make sure this is either in your path, or that you invoke it directly.  Open a command shell and navigate to your code from the Volatility repository.  Then type the following:

```
pyinstaller --onefile pyinstaller.spec
```

If all goes well, you should have an executable in a `dist` folder:

```
C:\Users\user\Desktop\volatility-master>dir dist

 Directory of C:\Users\user\Desktop\volatility-master\dist

08/25/2015  04:38 PM    <DIR>          .
08/25/2015  04:38 PM    <DIR>          ..
08/25/2015  04:56 PM        17,272,306 volatility.exe
               1 File(s)     17,272,306 bytes
               2 Dir(s)   5,143,740,416 bytes free
```

To verify that all libraries were included you can run the executable to test it.  You shouldn't see any import errors:

```
C:\Users\user\Desktop\volatility-master>dist\volatility.exe -h
Volatility Foundation Volatility Framework 2.4
Usage: Volatility - A memory forensics analysis platform.

Options:
  -h, --help            list all available options and their default values.
                        Default values may be set in the configuration file
[snip]
```