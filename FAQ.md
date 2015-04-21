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
    - [I'm getting an error: `[..] ( ImportError : No module named Crypto.Hash)`](FAQ#im-getting-an-error---importerror--no-module-named-cryptohash)
    - [Volatility thinks my image is invalid](FAQ#volatility-thinks-my-image-is-invalid)
    - [No address space mapping, No valid DTB found](FAQ#no-address-space-mapping-no-valid-dtb-found)
    - [Scanning commands report false positives](FAQ#scanning-commands-report-false-positives)
    - [How can I report a bug or feature request](FAQ#how-can-i-report-a-bug-or-feature-request)

- *Contact*
    - [Who wrote/is writing Volatility](FAQ#who-wroteis-writing-volatility)
    - [How can I contribute to Volatility](FAQ#how-can-i-contribute-to-volatility)

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
- The book [The Art of Memory Forensics](http://www.memoryanalysis.net/#!amf/cmg5) extensively covers the topic of memory analysis as well as Volatility internals.

### I'm getting an error: `[..] ( ImportError : No module named Crypto.Hash)`

This error occurs when [PyCrypto](https://www.dlitz.net/software/pycrypto/) is not installed. This is a library that is used by some of the registry plugins like lsadump. You will see this error message when using any of the plugins, however. If you are not using lsadump, hashdump or any other registry plugin that uses PyCrypto, then you can safely ignore the error message. Otherwise, install PyCrypto and the message will disappear. 

### Volatility thinks my image is invalid

If you run into the message "Could not list tasks, please verify the --profile option and whether this image is valid" there are a few things you should know. First, the --profile parameter should be set to the name of a Volatility profile that matches the OS and architecture of the memory dump. If you don't know which OS your memory dump came from, try using the imageinfo plugin for suggestions. If you use those suggestions and still see the error message, the most likely cause is multiple KDBG signatures. Volatility finds and uses the first KDBG signature it finds, which in the case of multiple KDBG signatures - the first one may not be the correct one. In this case, you should use the kdgbscan plugin and select an alternate KDBG address. When you run commands, also supply the --kdbg parameter. Here's an example walk-through:
```
$ python vol.py -f mem.dmp --profile=Win2003SP2x64 pslist 
Volatile Systems Volatility Framework 2.1_alpha
 Offset(V)  Name                 PID    PPID   Thds   Hnds   Time 
---------- -------------------- ------ ------ ------ ------ -------------------
Could not list tasks, please verify the --profile option and whether this image is valid
```
Now let's confirm the profile we're using is one that Volatility suggests:
```
$ python vol.py -f mem.dmp imageinfo 
Volatile Systems Volatility Framework 2.1_alpha
Determining profile based on KDBG search...

          Suggested Profile(s) : Win2003SP2x64, WinXPSP1x64
                     AS Layer1 : AMD64PagedMemory (Kernel AS)
                     AS Layer2 : FileAddressSpace (/Users/Michael/mem.dmp)
                      PAE type : PAE
                           DTB : 0x529000
                          KDBG : 0xf80001172cb0
                          KPCR : 0xffdff000
             KUSER_SHARED_DATA : 0xfffff78000000000L
           Image date and time : 2012-01-30 18:55:54 
     Image local date and time : 2012-01-30 18:55:54 
Could not list tasks, please verify the --profile option and whether this image is valid
```
Volatility suggested two profiles, the first and thus most likely profile is Win2003SP2x64 (which is the one we originally used). The KDBG signature was found at 0xf80001172cb0. Now let's double check if there are multiple KDBG signatures.
```
$ python vol.py -f mem.dmp kdbgscan --profile=Win2003SP2x64
Volatile Systems Volatility Framework 2.1_alpha
Potential KDBG structure addresses (P = Physical, V = Virtual):
 _KDBG: V 0xf80001172cb0  (Win2003SP2x64)
 _KDBG: P 0x01172cb0  (Win2003SP2x64)
 _KDBG: V 0xf80001175cf0  (Win2003SP2x64)
 _KDBG: P 0x01175cf0  (Win2003SP2x64)
```
Near the end you can see there's an alternate KDBG signature found at 0xf80001175cf0. Supply this one to --kdbg with using pslist and see if that solves the problem:
```
$ python vol.py -f mem.dmp --profile=Win2003SP2x64 --kdbg=0xf80001175cf0
Volatile Systems Volatility Framework 2.1_alpha
 Offset(V)  Name                 PID    PPID   Thds   Hnds   Time 
---------- -------------------- ------ ------ ------ ------ ------------------- 
0xfffffadfe7a7dc20 System                    4      0     60    341 1970-01-01 00:00:00       
0xfffffadfe736b040 smss.exe                300      4      3     19 2012-01-23 18:19:44       
0xfffffadfe6fe0c20 csrss.exe               348    300     13    449 2012-01-23 18:19:46       
0xfffffadfe7054c20 winlogon.exe            372    300     23    618 2012-01-23 18:19:47       
0xfffffadfe7115c20 services.exe            420    372     16    277 2012-01-23 18:19:50     
....
```

### No address space mapping, No valid DTB found

If you see the message "No suitable address space mapping found" and/or "No valid DTB found" most likely you've selected an invalid profile for the memory image you're analyzing. For example:
```
$ python vol.py -f win2k3sp0.vmem psscan
Volatile Systems Volatility Framework 2.1_alpha
 Offset(P)  Name             PID    PPID   PDB        Time created             Time exited             
---------- ---------------- ------ ------ ---------- ------------------------ ------------------------ 
No suitable address space mapping found
Tried to open image as:
 WindowsHiberFileSpace32: No base Address Space
 EWFAddressSpace: No libEWF implementation found
 WindowsCrashDumpSpace32: No base Address Space
 AMD64PagedMemory: No base Address Space
 JKIA32PagedMemory: No base Address Space
 JKIA32PagedMemoryPae: No base Address Space
 IA32PagedMemoryPae: Module disabled
 IA32PagedMemory: Module disabled
 WindowsHiberFileSpace32: No xpress signature found
 EWFAddressSpace: No libEWF implementation found
 WindowsCrashDumpSpace32: Header signature invalid
 AMD64PagedMemory: Incompatible profile WinXPSP2x86 selected
 JKIA32PagedMemory: No valid DTB found
 JKIA32PagedMemoryPae: No valid DTB found
 IA32PagedMemoryPae: Module disabled
 IA32PagedMemory: Module disabled
 FileAddressSpace: Must be first Address Space
```

You should check with the imageinfo plugin for profile suggestions (also see the previous FAQ entry). In this case, simply supplying the correct profile will fix the issue:

```
$ python vol.py -f win2k3sp0.vmem --profile=Win2003SP0x86 psscan
Volatile Systems Volatility Framework 2.1_alpha
 Offset(P)  Name             PID    PPID   PDB        Time created             Time exited             
---------- ---------------- ------ ------ ---------- ------------------------ ------------------------ 
0x01fc6550 vmtoolsd.exe       2816    508 0x0e717000 2010-09-26 20:15:24      2010-09-26 20:15:46     
0x01fca2f0 VMwareTray.exe     2788    152 0x098bc000 2010-09-26 20:15:24      2010-09-26 20:15:44     
0x01ff0d00 wpabaln.exe        3352    464 0x0d716000 2010-09-26 20:15:32      2010-09-26 20:15:44     
0x01ffed88 rundll32.exe       2704   2696 0x16c38000 2010-09-26 20:15:20      2010-09-26 20:15:44     
0x01fff348 spoolsv.exe        2096    508 0x1168f000 2010-09-26 20:14:55      2010-09-26 20:15:46     
0x02023ca0 msiexec.exe        1748   1920 0x079d4000 2010-09-26 20:14:51      2010-09-26 20:15:39
....
```

### Scanning commands report false positives

Commands like psscan, modscan, connscan, etc. use pool tag scanning to find objects (either active or residual) in physical memory. Thus Volatility scans over your entire memory dump looking for 4 byte pool tag signatures and then applies a serious of sanity checks (specific per object type). If you believe one of the scanners has found a false positive, you can investigate the reason using volshell (look for the section that describes how to use the dt() command with a physical address space). You also may want to view the source code for the plugin you're running and examine the sanity checks Volatility uses. In particular, look for classes that subclass scan.ScannerCheck? and view the list of checks in classes that subclass scan.PoolScanner. For more information on the scanning framework, see Scanners. 

### How can I report a bug or feature request

See [contact](FAQ#how-can-i-contribute-to-volatility)

### Who wrote/is writing Volatility

See the AUTHORS.txt and CREDITS.txt files provided with the Volatility source code.

### How can I contribute to Volatility

If you have documentation, code or ideas to contribute, use one of the following methods:

- Through the [Github interface](https://github.com/volatilityfoundation/volatility/issues)
- Through IRC: #volatility on freenode
- Through the [Volatility Mailing List](http://lists.volatilesystems.com/mailman/listinfo/vol-users) 