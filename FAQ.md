# Frequently Asked Questions

- *General* 
    - [What is the latest stable version of Volatility](FAQ#what-is-the-latest-stable-version-of-volatility)
    - [What is the latest development version of Volatility](FAQ#what-is-the-latest-development-version-of-volatility)
    - [What's new in 2.4](FAQ#whats-new-in-24)
    - [What operating systems does Volatility support](FAQ#what-operating-systems-does-volatility-support)
    - [What about reading crash dumps and hibernation files](FAQ#what-about-reading-crash-dumps-and-hibernation-files)
    - [Can Volatility acquire physical memory](FAQ#can-volatility-acquire-physical-memory)
    - [What's the largest memory dump Volatility can read](FAQ#whats-the-largest-memory-dump-volatility-can-read)
    - [Are there any public memory samples available that I can use for testing](FAQ#are-there-any-public-memory-samples-available-that-i-can-use-for-testing)

- *Installation*
    - [What are the dependencies for running Volatility](FAQ#what-are-the-dependencies-for-running-volatility)

- *Usage*
    - [How do I run Volatility](FAQ#how-do-i-run-volatility)
    - [How can I run external plugins with the standalone executable](FAQ#how-can-i-run-external-plugins-with-the-standalone-executable)
    - [Where can I find additional documentation on Volatility](FAQ#where-can-i-find-additional-documentation-on-volatility)

- *Troubleshooting*
    - [I'm getting an error: `[..] ( ImportError : No module named Crypto.Hash)`]
    - Volatility thinks my image is invalid
    - No address space mapping, No valid DTB found
    - Scanning commands report false positives
    - How can I report a bug or feature request

- *Contact*
    - Who wrote/is writing Volatility
    - How can I contribute to Volatility

### What is the latest stable version of Volatility

The latest stable version is 2.4. You can grab the source code, Python installer, or Windows standalone executable from the [downloads page](http://www.volatilityfoundation.org/#!releases/component_71401)

### What is the latest development version of Volatility

The latest development version is 2.4 which you can get by checking out the main branch using git (`$ git clone git@github.com:volatilityfoundation/volatility.git`). 

### What's new in 2.4

### What operating systems does Volatility support

### What about reading crash dumps and hibernation files

Volatility should automatically determine whether you've asked it to analyze a crash dump file or a hiberation file, and allow you to run plugins against them just like normal.

If you'd like to save these files as raw dd files, you can use the [imagecopy](Command Reference#imagecopy) plugin to convert them to raw memory images. The raw memory images can also be analyzed using the normal Volatility commands. 

### Can Volatility acquire physical memory

Short answer: No. Long Answer: The [imagecopy](Command Reference#imagecopy) plugin can be used to copy one address space to a file (allowing acquisition from address spaces such as IEEE 1394), but in general no, to acquire memory, you must use another tool. For a list of possibilities, see: [The Forensics Wiki](http://www.forensicswiki.org/wiki/Tools:Memory_Imaging)

### What's the largest memory dump Volatility can read

There is technically no limit. We've heard reports of Volatility handling 30-40 GB images on both Windows and Linux host operating systems. If you routinely analyze large memory dumps and would like to supply some performance benchmarks for the FAQ, please let us know. 

### Are there any public memory samples available that I can use for testing

Yes.  Check the [memory samples wiki](https://github.com/volatilityfoundation/volatility/wiki/Memory-Samples)

### What are the dependencies for running Volatility

Note: once the installers are released, update this section since some installers come pre-packaged with python and the dependencies.

- Here is what you need for the core functionality:
    - A Windows, Linux, or Mac OS X machine
    - Python version 2.6 or greater (but not 3.x) 

- Some plugins require third party libraries which you can get here:
    - [Distorm3](https://code.google.com/p/distorm/) (Malware Plugins, Volshell)
    - [Yara](http://plusvic.github.io/yara/) (Malware Plugins)
    - [PyCrypto](https://www.dlitz.net/software/pycrypto/) (Core)
    - [OpenPyxl](http://pythonhosted.org/openpyxl/) (Timeliner)

### How do I run Volatility

See [basic usage](Volatility-Usage)

### How can I run external plugins with the standalone executable

With the standalone executable you have to specify the location of external plugins directory or zipfile using the "--plugins" switch. In general you can specify the path of the item "--plugins=<path to directory/zipfile>". An example using a directory called "plugins" can be seen below:
```
C:\vol>volatility.exe --plugins=..\plugins malfind -f c:\memory_images\win7.dd --profile=Win7SP0x86 -D output
Volatile Systems Volatility Framework 2.0 
Name                 Pid    Start      End        Tag      Hits   Protect
svchost.exe          740    0x005b0000 0x5f0fff00 Vad      0      PAGE_EXECUTE_WRITECOPY
Dumped to: output\svchost.exe.3e3949d0.005b0000-005f0fff.dmp
0x005b0000   4d 5a 90 00 03 00 00 00 04 00 00 00 ff ff 00 00    MZ..............
0x005b0010   b8 00 00 00 00 00 00 00 40 00 00 00 00 00 00 00    ........@.......
0x005b0020   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
0x005b0030   00 00 00 00 00 00 00 00 00 00 00 00 d8 00 00 00    ................
0x005b0040   0e 1f ba 0e 00 b4 09 cd 21 b8 01 4c cd 21 54 68    ........!..L.!Th
0x005b0050   69 73 20 70 72 6f 67 72 61 6d 20 63 61 6e 6e 6f    is program canno
0x005b0060   74 20 62 65 20 72 75 6e 20 69 6e 20 44 4f 53 20    t be run in DOS
0x005b0070   6d 6f 64 65 2e 0d 0d 0a 24 00 00 00 00 00 00 00    mode....$.......
[snip]
```
You can also give Volatility a zipfile containing plugins:
```
C:\vol>volatility.exe --plugins=malfind.zip malfind -f c:\memory_images\win7.dd --profile=Win7SP0x86 -D output
Volatile Systems Volatility Framework 2.0 
Name                 Pid    Start      End        Tag      Hits   Protect
svchost.exe          740    0x005b0000 0x5f0fff00 Vad      0      PAGE_EXECUTE_WRITECOPY
Dumped to: output\svchost.exe.3e3949d0.005b0000-005f0fff.dmp
0x005b0000   4d 5a 90 00 03 00 00 00 04 00 00 00 ff ff 00 00    MZ..............
0x005b0010   b8 00 00 00 00 00 00 00 40 00 00 00 00 00 00 00    ........@.......
0x005b0020   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
0x005b0030   00 00 00 00 00 00 00 00 00 00 00 00 d8 00 00 00    ................
0x005b0040   0e 1f ba 0e 00 b4 09 cd 21 b8 01 4c cd 21 54 68    ........!..L.!Th
0x005b0050   69 73 20 70 72 6f 67 72 61 6d 20 63 61 6e 6e 6f    is program canno
0x005b0060   74 20 62 65 20 72 75 6e 20 69 6e 20 44 4f 53 20    t be run in DOS
0x005b0070   6d 6f 64 65 2e 0d 0d 0a 24 00 00 00 00 00 00 00    mode....$.......
[snip]
```
Note: Due to the way plugins are loaded, the external plugins directory or zipfile must be specified before any plugin-specific arguments (including the name of the plugin). 

### Where can I find additional documentation on Volatility

See the following resources:

- The [Volatility Documentation Project Wiki](Volatility-Documentation-Project) contains links to external web sites.
- [Malware Analyst's Cookbook](http://www.malwarecookbook.com/) devotes 4 chapters to using Volatility for malware analysis. 

 ### I'm getting an error: `[..] ( ImportError : No module named Crypto.Hash)`