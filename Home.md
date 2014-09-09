### Quick Start

 * *Choose a release* - the most recent is [Volatility 2.4] (http://www.volatilityfoundation.org/#!24/c12wa), released August 2014. Older versions are also available on the [Releases](http://www.volatilityfoundation.org/#!releases/component_71401) page or respective release pages. If you want the cutting edge development build, use a git client and clone the master.
 * *Install the code* - Volatility is packaged in several formats, including source code in zip or tar archive (all platforms), a Pyinstaller executable (Windows only) and a standalone executable (Windows only). For help deciding which format is best for your needs, and for installation or upgrade instructions, see [Installation](Installation).
 * *Target OS specific setup* - the Linux, Mac, and Andoid support may require accessing symbols and building your own profiles before using Volatility. If you plan to analyze these operating systems, please see [Linux](Linux), [Mac](Mac), or [Android](Android).
 * *Read usage and plugins* - command-line parameters, options, and plugins may differ between releases. For the most recent information, see [[Volatility Usage]] and [[Command Reference]]. 
 * *Communicate* - If you have documentation, patches, ideas, or bug reports, you can communicate them through the  [github interface](https://github.com/volatilityfoundation/volatility/issues), IRC (#volatility on freenode), the [Volatility Mailing List](http://lists.volatilesystems.com/mailman/listinfo) or Twitter ([@volatility](https://twitter.com/volatility)). 
 * *Develop* - For advanced users who want to develop their own plugins, address spaces, and other components of volatility, there is a recommended [StyleGuide](https://github.com/volatilityfoundation/volatility/wiki/Style-Guide).

### Malware and Memory Forensics Training 

We've put together an exhaustive course covering everything you need to know about memory forensics for malware investigations, incident response, and digital forensics. The material is "field tested" and has been executed in front of hundreds of students.

For more information, click the link for the event you're interested in or read [student testimonials](http://www.memoryanalysis.net/#!testimonials/c1dos).

Current Courses:

* [Feb 2015 in São Paulo, Brazil](http://www.memoryanalysis.net/#!New-Event-in-So-Paulo-Brazil-Feburary-2nd---6th-2015/c1zo4/BFB221C1-986F-4839-BD6A-D61FFAAFFFD0)
* [Jan 2015 in San Francisco, CA](http://www.memoryanalysis.net/#!New-Event-in-San-Francisco-CA-January-12th---16th-2015/c1zo4/CFA6C04F-F874-4FA5-A9CF-087E3141BB2A)
* [Dec 2014 in Austin, TX](http://www.memoryanalysis.net/#!New-Event-in-Austin-TX-December-8th---12th-2014/c1zo4/EF551588-9E8A-4036-B5B5-4E4B5D4A4D66)
* [Oct 2014 in Reston, VA](http://www.memoryanalysis.net/#!New-Event-in-Reston-VA-October-6th---10th-2014/c1zo4/FE9134FC-B2E4-47AF-A2F1-D3899840A4B7)

Past Courses:

* [Aug 2014 in Canberra, AU](http://www.memoryanalysis.net/#!New-Event-in-Canberra-AU-August-25th---29th-2014/c1zo4/5F514FDF-F43B-415D-9053-84E9A5E3FE3A)
* [Jun 2014 in London, UK](http://www.memoryanalysis.net/#!New-Event-in-London-UK-June-9th---13th-2014/c1zo4/0BB86C58-D266-4A49-B503-5EA917057394)
* [May 2014 in New York, NY](http://www.memoryanalysis.net/#!New-Event-in-New-York-NY-May-5th---9th-2014/c1zo4/1)
* [Jan 2014 in San Diego CA](http://volatility-labs.blogspot.com/2013/09/2014-malware-and-memory-forensics.html)
* [Nov 2013 in Reston VA](http://volatility-labs.blogspot.com/2013/06/memory-forensics-training-reston-va.html)
* [Sep 2013 in The Netherlands](http://volatility-labs.blogspot.com/2013/04/memory-forensics-training-netherlands.html)
* [Jun 2013 in Reston, VA](http://volatility-labs.blogspot.com/2013/03/official-training-by-volatility.html)
* [Mar 2013 in Chicago, IL](http://volatility-labs.blogspot.com/2013/01/windows-malware-and-memory-forensics.html)
* [Dec 2012 in Reston, VA](http://volatility-labs.blogspot.com/2012/11/windows-memory-forensics-training-for.html)

### Why Volatility

  * **A single, cohesive framework** analyzes RAM dumps from 32- and 64-bit windows, linux, mac, and android systems. Volatility's modular design allows it to easily support new operating systems and architectures as they are released. All your devices are targets...so don't limit your forensic capabilities to just windows computers. 


[[images/windows.jpeg]][[images/apple.png]][[images/linux.png]][[images/android.gif]]

  * **Its Open Source GPLv2**, which means you can read it, learn from it, and extend it. Why use a tool that outputs results without giving you any indication where the values came from or how they were interpreted? Learn how your tools work, understand why and how to tweak and enhance them - help yourself become a smarter analyst. You can also immediately fix any issues you discover, instead of having to wait weeks or months for vendors to communicate, reproduce, and publish patches.
  * **Its written in Python**, an established forensic and reverse engineering language with loads of libraries that can easily integrate into volatility. Most analysts are already familiar with Python and don't want to learn new languages. For example, windbg's scripting syntax which is often seen as cryptic and many times the capabilities just aren't there. Other memory analysis frameworks require you to use Visual Studio to compile C# DLLs and the rest don't expose a programming API at all.  
  * **Runs on windows, linux, or mac** analysis systems (anywhere Python runs) - a refreshing break from other memory analysis tools that only run on windows and require .NET installations and admin privileges just to open. If you're already accustomed to performing forensics on a particular host OS, by all means keep using it - and take volatility with you. 
  * **Extensible and scriptable API** gives you the power to go beyond and continue innovating. For example you can use volatility to build a customized web interface or GUI, drive your malware sandbox, perform virtual machine introspection or just explore kernel memory in an automated fashion. Analysts can add new address spaces, plugins, data structures, and overlays to truly weld the framework to their needs. 
  * **Unparalleled feature sets** based on reverse engineering and specialized research. Volatility provides capabilities that Microsoft's own kernel debugger doesn't allow, such as carving command histories, console input/output buffers, USER objects (GUI memory), and network related data structures. Just because its not documented doesn't mean you can't analyze it! 
  * **Comprehensive coverage of file formats** - volatility can analyze raw dumps, crash dumps, hibernation files, VMware .vmem, VMware saved state and suspended files (.vmss/.vmsn), VirtualBox core dumps, LiME (Linux Memory Extractor), expert witness (EWF), and direct physical memory over Firewire. You can even convert back and forth between these formats. In the heat of your incident response moment, don't get caught looking like a fool when someone hands you a format your other tools can't parse. 
  * **Fast and efficient algorithms** let you analyze RAM dumps from large systems without unnecessary overhead or memory consumption. For example volatility is able to list kernel modules from an 80 GB system in just a few seconds. There is always room for improvement, and timing differs per command, however other memory analysis frameworks can take several hours to do the same thing on much smaller memory dumps.
  * **Serious and powerful community** of practitioners and researchers who work in the forensics, IR, and malware analysis fields. It brings together contributors from commercial companies, law enforcement, and academic institutions around the world. Don't just take our word for it - check out the [Volatility-Documentation-Project](Volatility-Documentation-Project) - a collection of over 200 docs from 60+ different authors. Volatility is also being built on by a number of large organizations such as Google, National DoD Laboratories, DC3, and many Antivirus and security shops.
  * **Forensics/IR/malware focus** - Volatility was designed by forensics, incident response, and malware experts to focus on the types of tasks these analysts typically form. As a result, there are things that are often very important to a forensics analysts that are not as important to a person debugging a kernel driver (unallocated storage, indirect artifacts, etc). 
  * **Money-back guarantee** - although volatility is free, we stand by our work. There is nothing another memory analysis framework can do that volatility can't (or that it can't be quickly programmed to do).