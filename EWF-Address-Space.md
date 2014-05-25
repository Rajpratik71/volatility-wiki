Memory acquired by EnCase or converted using ewfacquire are stored in Expert Witness Format (EWF).  Volatility supports the "older" EWF format used by EnCase v6 (and prior versions), but not the newer EWF2-EX01 format used in EnCase v7.

# Acquisition

Click on "Add Device" in EnCase and then make sure that "Physical Memory" is checked.  Depending on your version of EnCase (EE for example), the folders may differ below.

After hitting "Next" you should see RAM for the requested machine as an option.  "Blue-check" it and hit "Next".

[[images/EnCase_RAM.png]]

Then hit "Finish"

You should see the RAM in your evidence entries window.

[[images/EnCase_RAM_View.png]]


To acquire the sample, right-click, click on "Acquire"  and follow the acquisition dialog that follows.

[[images/EnCase_RAM_Acquisition.png]]

# Notes

You must have libewf installed for the EWF address space to work correctly.  The address space can be found in the `contrib/plugins/aspaces` folder.  You can use the `--plugins=` parameter in order to use the ewf.py address space without moving it.  The `--plugins=` parameter must come before any other parameters for `vol.py`.  You can see an example below:

	$ python vol.py --plugins=contrib/plugins -f WinXPSP3x86.E01 --profile=WinXPSP3x86 pslist
	Volatility Foundation Volatility Framework 2.4
	Offset(V)  Name                    PID   PPID   Thds     Hnds   Sess  Wow64 Start                Exit    
	---------- -------------------- ------ ------ ------ -------- ------ ------ -------------------- --------------------
	0x8aeda660 System                    4      0     99     2022 ------      0    
	0x89af3da0 smss.exe                912      4      3       19 ------      0 2011-04-08 17:30:59    
	0x894c3720 csrss.exe              1036    912     14     1086      0      0 2011-04-08 17:31:05    
	0x894ceda0 winlogon.exe           1060    912     22      604      0      0 2011-04-08 17:31:07    
	0x86ff4da0 services.exe           1108   1060     16      417      0      0 2011-04-08 17:31:10    
	0x8705a770 lsass.exe              1120   1060     23      531      0      0 2011-04-08 17:31:10    
	0x86fdbda0 svchost.exe            1368   1108     16      208      0      0 2011-04-08 17:31:12    
	[snip] 

# File Format

File format details can be found in [Joachim Metz's EWF documentation](http://code.google.com/p/libewf/downloads/detail?name=Expert%20Witness%20Compression%20Format%20%28EWF%29.pdf).

