**Table of Contents**  

- [Introduction](Volatility-2.3#introduction)
- [Release Highlights](Volatility-2.3#release-highlights)
- [Operating Systems](Volatility-2.3#operating-systems)
- [Address Spaces](Volatility-2.3#address-spaces)
- [Plugins](Volatility-2.3#plugins)
- [Credits](Volatility-2.3#credits)

# Introduction 

This page contains the release notes for Volatility 2.3.

# Release Highlights  

 * Windows 
    * new plugins to parse IE history/index.dat URLs, recover shellbags data, dump cached files (exe/pdf/doc/etc), extract the MBR and MFT records, explore recently unloaded kernel modules, dump SSL private and public keys/certs, and display details on process privileges
    * added plugins to detect poison ivy infections, find and decrypt configurations in memory for poison ivy, zeus v1, zeus v2 and citadelscan 1.3.4.5 
    * apihooks detects duqu style instruction modifications (MOV reg32, imm32; JMP reg32)
    * crashinfo displays uptime, systemtime, and dump type (i.e. kernel, complete, etc)
    * psxview plugin adds two new sources of process listings from the GUI APIs 
    * screenshots plugin shows text for window titles 
    * svcscan automatically queries the cached registry for service dlls
    * dlllist shows load count to distinguish between static and dynamic loaded dlls
 * New address spaces 
        * added support for !VirtualBox ELF64 core dumps, VMware saved state (vmss) and snapshot (vmsn) files, and FDPro's non-standard HPAK format 
        * associated plugins: vboxinfo, vmwareinfo, hpakinfo, hpakextract 
 * Mac 
    * new MachO address space for 32- and 64-bit Mac memory samples
    * over 30+ plugins for Mac memory forensics
 * Linux/Android
    * new ARM address space to support memory dumps from Linux and Android devices on ARM 
    * added plugins to scan linux process and kernel memory with yara signatures, dump LKMs to disk, and check TTY devices for rootkit hooks
    * added plugins to check the ARM system call and exception vector tables for hooks

# Operating Systems  

Volatility supports the following operating systems and versions. All Windows profiles are included in the standard Volatility package. You can download sample Linux profiles from the LinuxProfiles wiki page or read LinuxMemoryForensics on how to build your own. You can download a [single archive of 38 different Mac OSX profiles](https://github.com/gleeda/Volatility/archive/macprofiles.zip) or read MacMemoryForensics to build your own. 

 * Windows 
   * 32-bit Windows XP Service Pack 2 and 3
   * 32-bit Windows 2003 Server Service Pack 0, 1, 2
   * 32-bit Windows Vista Service Pack 0, 1, 2
   * 32-bit Windows 2008 Server Service Pack 1, 2 
   * 32-bit Windows 7 Service Pack 0, 1
   * 64-bit Windows XP Service Pack 1 and 2 
   * 64-bit Windows 2003 Server Service Pack 1 and 2 
   * 64-bit Windows Vista Service Pack 0, 1, 2
   * 64-bit Windows 2008 Server Service Pack 1 and 2 
   * 64-bit Windows 2008 `R2` Server Service Pack 0 and 1
   * 64-bit Windows 7 Service Pack 0 and 1
 * Linux 
   * 32-bit Linux kernels 2.6.11 to 3.5
   * 64-bit Linux kernels 2.6.11 to 3.5
   * OpenSuSE, Ubuntu, Debian, CentOS, Fedora, Mandriva, etc
 * Mac OSX 
   * <font color='red'>(new)</font> 32-bit 10.5.x Leopard (the only 64-bit 10.5 is Server, which isn't supported)
   * <font color='red'>(new)</font> 32-bit 10.6.x Snow Leopard 
   * <font color='red'>(new)</font> 64-bit 10.6.x Snow Leopard 
   * <font color='red'>(new)</font> 32-bit 10.7.x Lion 
   * <font color='red'>(new)</font> 64-bit 10.7.x Lion 
   * <font color='red'>(new)</font> 64-bit 10.8.x Mountain Lion (there is no 32-bit version)

# Address Spaces  

  * !FileAddressSpace - This is a direct file AS
  * Standard Intel x86 address spaces
      * IA32PagedMemoryPae
      * IA32PagedMemory
  * AMD64PagedMemory - This AS supports AMD 64-bit address spaces
  * [CrashAddressSpace WindowsCrashDumpSpace32] - This AS supports windows Crash Dump format (x86)
  * [CrashAddressSpace WindowsCrashDumpSpace64] - This AS supports windows Crash Dump format (x64)
  * [HiberAddressSpace WindowsHiberFileSpace32] - This AS supports windows hibernation files (x86 and x64)
  * [EWFAddressSpace EWFAddressSpace] - This AS supports expert witness (EWF) files
  * [FirewireAddressSpace FirewireAddressSpace] - This AS supports direct memory access over firewire
  * [LimeAddressSpace LimeAddressSpace] - This AS supports LiME (Linux Memory Extractor)
  * <font color='red'>(new)</font> [MachOAddressSpace MachOAddressSpace] - This AS supports 32- and 64-bit Mac OSX memory dumps
  * <font color='red'>(new)</font> [ArmAddressSpace ArmAddressSpace] - This AS supports memory dumps from 32-bit ARM (there is no 64-bit ARM yet)
  * <font color='red'>(new)</font> [VirtualBoxCoreDump VirtualBoxCoreDumpElf64] - This AS supports memory dumps from !VirtualBox virtual machines 
  * <font color='red'>(new)</font> [VMwareSnapshotFile VMware Snapshot] - This AS supports VMware saved state (.vmss) and VMware snapshot (.vmsn) files. *Note*: these are _not_ raw memory dumps like the typical .vmem files. 
  * <font color='red'>(new)</font> [HPAKAddressSpace HPAKAddressSpace] - This AS supports ".hpak" files produced by H.B. Gary's FDPro tool.

# Plugins  

  * *Windows* 
      * *Image Identification*
          * [Command-Reference23#imageinfo imageinfo] - Identify information for the image
          * [Command-Reference23#kdbgscan kdbgscan] - Search for and dump potential KDBG values
          * [Command-Reference23#kpcrscan kpcrscan] - Search for and dump potential `_KPCR` values
      * *Process and DLLs*
          * [Command-Reference23#pslist pslist] - Print active processes by following the `_EPROCESS` list
          * [Command-Reference23#pstree pstree] - Print process list as a tree
          * [Command-Reference23#psscan psscan] - Scan Physical memory for `_EPROCESS` pool allocations
          * [Command-Reference23#psdispscan psdispscan] - Scan Physical memory for `_EPROCESS` objects based on Dispatch Headers (Windows XP x86 only)
          * [Command-Reference23#dlllist dlllist] - Print list of loaded DLLs for each process
          * [Command-Reference23#dlldump dlldump] - Dump DLLs from a process address space
          * [Command-Reference23#handles handles] - Print list of open handles for each process
          * [Command-Reference23#getsids getsids] - Print the SIDs owning each process
          * [Command-Reference23#verinfo verinfo] - Print a PE file's version information 
          * [Command-Reference23#enumfunc enumfunc] - Enumerate a PE file's imports and exports
          * [Command-Reference23#envars envars] - Display process environment variables
          * [Command-Reference23#cmdscan cmdscan] - Extract command history by scanning for `_COMMAND_HISTORY`
          * [CommandReference21#consoles consoles] - Extract command history by scanning for `_CONSOLE_INFORMATION`
          * <font color='red'>(new)</font> [Command-Reference23#privs privs] - Identify the present and/or enabled windows privileges for each process
      * *Process Memory*
          * [Command-Reference23#memmap memmap] - Print the memory map
          * [Command-Reference23#memdump memdump] - Dump the addressable memory for a process
          * [Command-Reference23#procexedump procexedump] - Dump a process to an executable file
          * [Command-Reference23#procmemdump procmemdump] - Dump a process to an executable memory sample
          * [Command-Reference23#vadwalk vadwalk] - Walk the VAD tree
          * [Command-Reference23#vadtree vadtree] - Walk the VAD tree and display in tree format
          * [Command-Reference23#vadinfo vadinfo] - Dump the VAD info
          * [Command-Reference23#vaddump vaddump] - Dumps out the vad sections to a file
          * [Command-Reference23#evtlogs evtlogs] - Parse XP and 2003 event logs from memory
          * <font color='red'>(new)</font> [Command-Reference23#iehistory iehistory] - Extract and parse Internet Explorer history and URL cache 
      * *Kernel Memory and Objects*
          * [Command-Reference23#modules modules] - Print list of loaded modules
          * [Command-Reference23#modscan modscan] - Scan Physical memory for `_LDR_DATA_TABLE_ENTRY` objects
          * [Command-Reference23#moddump moddump] - Extract a kernel driver to disk
          * [Command-Reference23#ssdt ssdt] - Print the Native and GDI System Service Descriptor Tables
          * [Command-Reference23#driverscan driverscan] - Scan physical memory for `_DRIVER_OBJECT` objects
          * [Command-Reference23#filescan filescan] - Scan physical memory for `_FILE_OBJECT` objects
          * [Command-Reference23#mutantscan mutantscan] - Scan physical memory for `_KMUTANT` objects
          * [Command-Reference23#symlinkscan symlinkscan] - Scans for symbolic link objects
          * [Command-Reference23#thrdscan thrdscan] - Scan physical memory for `_ETHREAD` objects
          * <font color='red'>(new)</font> [Command-Reference23#dumpfiles dumpfiles] - Reconstruct files from the windows cache manager and shared section objects
          * <font color='red'>(new)</font> [Command-Reference23#unloadedmodules unloadedmodules] - Show recently unloaded kernel modules (which indirectly tells you which ones recently loaded)
      * *Win32k / GUI Memory*
          * [CommandReferenceGui23#sessions sessions] - List details on `_MM_SESSION_SPACE` (user logon sessions)
          * [CommandReferenceGui23#wndscan wndscan] - Pool scanner for tagWINDOWSTATION (window stations)
          * [CommandReferenceGui23#deskscan deskscan] - Poolscaner for tagDESKTOP (desktops)
          * [CommandReferenceGui23#atomscan atomscan] - Pool scanner for `_RTL_ATOM_TABLE`
          * [CommandReferenceGui23#atoms atoms] - Print session and window station atom tables
          * [CommandReferenceGui23#clipboard clipboard] - Extract the contents of the windows clipboard
          * [CommandReferenceGui23#eventhooks eventhooks] - Print details on windows event hooks
          * [CommandReferenceGui23#gahti gathi] - Dump the USER handle type information
          * [CommandReferenceGui23#messagehooks messagehooks] - List desktop and thread window message hooks
          * [CommandReferenceGui23#screenshot screenshot] - Save a pseudo-screenshot based on GDI windows
          * [CommandReferenceGui23#userhandles userhandles] - Dump the USER handle tables
          * [CommandReferenceGui23#windows windows] - Print Desktop Windows (verbose details)
          * [CommandReferenceGui23#wintree wintree] - Print Z-Order Desktop Windows Tree
          * [CommandReferenceGui23#gditimers gditimers] - Analyze GDI timer objects and their callbacks
      * *Networking*
          * [Command-Reference23#connections connections] - Print open connections (XP and 2003 only)
          * [Command-Reference23#connscan connscan] - Scan Physical memory for `_TCPT_OBJECT` objects (XP and 2003 only)
          * [Command-Reference23#sockets sockets] - Print open sockets (XP and 2003 only)
          * [Command-Reference23#sockscan sockscan] - Scan Physical memory for `_ADDRESS_OBJECT` (XP and 2003 only)
          * [Command-Reference23#netscan netscan] - Scan physical memory for network objects (Vista, 2008, and 7)
      * *Registry*
          * [Command-Reference23#hivescan hivescan] - Scan Physical memory for `_CMHIVE` objects
          * [Command-Reference23#hivelist hivelist] - Print list of registry hives
          * [Command-Reference23#printkey printkey] - Print a registry key, and its subkeys and values
          * [Command-Reference23#hivedump hivedump] - Recursively prints all keys and timestamps in a given hive
          * [Command-Reference23#hashdump hashdump] - Dumps passwords hashes (LM/NTLM) from memory (x86 only)
          * [Command-Reference23#lsadump lsadump] - Dump (decrypted) LSA secrets from the registry (XP and 2003 x86 only)
          * [Command-Reference23#userassist userassist] - Parses and output User Assist keys from the registry
          * [CommandReference21#shimcache shimcache] - Parses the Application Compatibility Shim Cache registry key
          * [Command-Reference23#getservicesids getservicesids] - Calculate SIDs for windows services in the registry
          * <font color='red'>(new)</font> [Command-Reference23#shellbags shellbags] - This plugin parses and prints Shellbag information obtained from the registry
      * *File Formats*
          * [Command-Reference23#crashinfo crashinfo] - Dump crash-dump information
          * [Command-Reference23#hibinfo hibinfo] - Dump hibernation file information
          * [Command-Reference23#imagecopy imagecopy] - Copies a physical address space out as a raw DD image
          * [Command-Reference23#raw2dmp raw2dmp] - Converts a physical memory sample to a windbg crash dump
          * <font color='red'>(new)</font> [Command-Reference23#vboxinfo vboxinfo] - Display header and memory runs information from !VirtualBox core dumps
          * <font color='red'>(new)</font> [Command-Reference23#vmwareinfo vmwareinfo] - Display header and memory runs information from VMware vmss or vmsn files
          * <font color='red'>(new)</font> [Command-Reference23#hpakinfo hpakinfo] - Display header and memory runs information from .hpak files 
          * <font color='red'>(new)</font> [Command-Reference23#hpakextract hpakextract] - Extract (and decompress if necessary) the raw physical memory dump from an .hpak file 
      * *Malware*
          * [CommandReferenceMal23#malfind malfind] - Find hidden and injected code
          * [CommandReferenceMal23#svcscan svcscan] - Scan for Windows services
          * [CommandReferenceMal23#ldrmodules ldrmodules] - Detect unlinked DLLs
          * [CommandReferenceMal23#impscan impscan] - Scan for calls to imported functions
          * [CommandReferenceMal23#apihooks apihooks] - Detect API hooks in process and kernel memory (x86 only)
          * [CommandReferenceMal23#idt idt] - Dumps the Interrupt Descriptor Table (x86 only)
          * [CommandReferenceMal23#gdt gdt] - Dumps the Global Descriptor Table (x86 only)
          * [CommandReferenceMal23#threads threads] - Investigate `_ETHREAD` and `_KTHREAD`s
          * [CommandReferenceMal23#callbacks callbacks] - Print system-wide notification routines (x86 only)
          * [CommandReferenceMal23#driverirp driverirp] - Driver IRP hook detection
          * [CommandReferenceMal23#devicetree devicetree] - Show device tree
          * [CommandReferenceMal23#psxview psxview] - Find hidden processes with various process listings
          * [CommandReferenceMal23#timers timers] - Print kernel timers and associated module DPCs (x86 only)
      * *File System*
          * <font color="red">(new)</font> [Command-Reference23#mbrparser mbrparser] - Scans for and parses potential Master Boot Records (MBRs)
          * <font color="red">(new)</font> [Command-Reference23#mftparser mftparser] - Scans for and parses potential MFT entries
      * *Miscellaneous*
          * [Command-Reference23#strings strings] - Match physical offsets to virtual addresses
          * [Command-Reference23#volshell volshell] - Shell to interactively explore a memory image
          * [Command-Reference23#bioskbd bioskbd] - Reads the keyboard buffer from Real Mode memory
          * [Command-Reference23#patcher patcher] - Patches memory based on page scans
          * <font color="red">(new)</font> [Command-Reference23#timeliner timeliner] - Produce timelines in body file format, excel 2007 spreadsheets, or text 
          * <font color="red">(new)</font> [Command-Reference23#dumpcerts dumpcerts] - Extract SSL private and public keys/certs 
  * *Linux/Android*
      * *Processes*
          * [LinuxCommand-Reference23#linux_pslist linux_pslist] - Gather active tasks by walking the task_struct->task list
          * [LinuxCommand-Reference23#linux_psaux linux_psaux] - Gathers processes along with full command line and start time
          * [LinuxCommand-Reference23#linux_pstree linux_pstree] - Shows the parent/child relationship between processes
          * [LinuxCommand-Reference23#linux_pslist_cache linux_pslist_cache] - Gather tasks from the kmem_cache
          * [LinuxCommand-Reference23#linux_pidhashtable linux_pidhashtable] - Enumerates processes through the PID hash table
          * [LinuxCommand-Reference23#linux_psxview linux_psxview] - Find hidden processes with various process listings
          * [LinuxCommand-Reference23#linux_lsof linux_lsof] - Lists open files
      * *Process Memory*
          * [LinuxCommand-Reference23#linux_memmap linux_memmap] - Dumps the memory map for linux tasks
          * [LinuxCommand-Reference23#linux_proc_maps linux_proc_maps] - Gathers process maps for linux
          * [LinuxCommand-Reference23#linux_dump_map linux_dump_map] - Writes selected process memory mappings to disk
          * [LinuxCommand-Reference23#linux_bash linux_bash] - Recover bash history from bash process memory
      * *Kernel Memory and Objects*
          * [LinuxCommand-Reference23#linux_lsmod linux_lsmod] - Gather loaded kernel modules
          * [LinuxCommand-Reference23#linux_tmpfs linux_tmpfs] - Recovers tmpfs filesystems from memory
          * <font color='red'>(new)</font> [LinuxCommand-Reference23#linux_moddump linux_moddump] - Extract an LKM from memory to disk (.text segment only)
      * *Networking*
          * [LinuxCommand-Reference23#linux_arp linux_arp] - Print the ARP table
          * [LinuxCommand-Reference23#linux_ifconfig linux_ifconfig] - Gathers active interfaces
          * [LinuxCommand-Reference23#linux_netstat linux_netstat] - Lists open sockets
          * [LinuxCommand-Reference23#linux_route_cache linux_route_cache] - Recovers the routing cache from memory
          * [LinuxCommand-Reference23#linux_pkt_queues linux_pkt_queues] - Writes per-process packet queues out to disk
          * [LinuxCommand-Reference23#linux_sk_buff_cache linux_sk_buff_cache] - Recovers packets from the sk_buff kmem_cache
      * *Malware/Rootkits*
          * [LinuxCommand-Reference23#linux_check_afinfo linux_check_afinfo] - Verifies the operation function pointers of network protocols
          * [LinuxCommand-Reference23#linux_check_creds linux_check_creds] - Checks if any processes are sharing credential structures
          * [LinuxCommand-Reference23#linux_check_fop linux_check_fop] - Check file operation structures for rootkit modifications
          * [LinuxCommand-Reference23#linux_check_idt linux_check_idt] - Checks if the IDT has been altered
          * [LinuxCommand-Reference23#linux_check_modules linux_check_modules] - Compares module list to sysfs info, if available
          * [LinuxCommand-Reference23#linux_check_syscall linux_check_syscall] - Checks if the system call table has been altered
          * <font color='red'>(new)</font> [LinuxCommand-Reference23#linux_check_syscall_arm linux_check_syscall_arm] - Checks if the system call table has been altered (ARM)
          * <font color='red'>(new)</font> [LinuxCommand-Reference23#linux_check_tty linux_check_tty] - Check TTY devices for rootkit hooks
          * <font color='red'>(new)</font> [LinuxCommand-Reference23#linux_check_evt_arm linux_check_evt_arm] - Check ARM exception vector table for hooks
      * *System Information*
          * [LinuxCommand-Reference23#linux_cpuinfo linux_cpuinfo] - Prints info about each active processor
          * [LinuxCommand-Reference23#linux_dmesg linux_dmesg] - Gather dmesg buffer
          * [LinuxCommand-Reference23#linux_iomem linux_iomem] - Provides output similar to /proc/iomem
          * [LinuxCommand-Reference23#linux_mount linux_mount] - Gather mounted fs/devices
          * [LinuxCommand-Reference23#linux_mount_cache linux_mount_cache] - Gather mounted fs/devices from kmem_cache
          * [LinuxCommand-Reference23#linux_slabinfo linux_slabinfo] - Mimics /proc/slabinfo on a running machine    
          * [LinuxCommand-Reference23#linux_dentry_cache linux_dentry_cache] - Gather files from the dentry cache  
          * [LinuxCommand-Reference23#linux_find_file linux_find_file] - Extract cached file contents from memory via inodes 
          * [LinuxCommand-Reference23#linux_vma_cache linux_vma_cache] - Gather VMAs from the vm_area_struct cache
          * <font color='red'>(new)</font> [LinuxCommand-Reference23#linux_keyboard_notifier linux_keyboard_notifier] - Parses the keyboard notifier call chain
      * *Miscellaneous*
          * <font color='red'>(new)</font> [Command-Reference23#linux_volshell linux_volshell] - Shell to interactively explore Linux/Android memory captures
          * <font color='red'>(new)</font> [Command-Reference23#linux_yarascan linux_yarascan] - Scan process and kernel memory with yara signatures
  * *Mac OSX*
      * *Processes*
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_pslist mac_pslist] - List running processes
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_tasks mac_tasks] - List active tasks 
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_pstree mac_pstree] - Show parent/child relationship of processes
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_lsof mac_lsof] - Lists per-process open files
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_pgrp_hash_table mac_pgrp_hash_table] - Walks the process group hash table
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_pid_hash_table mac_pid_hash_table] - Walks the pid hash table
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_dead_procs mac_dead_procs] - List dead/terminated processes
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_psaux mac_psaux] - Prints processes with their command-line arguments (argv)
      * *Process Memory*
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_proc_maps mac_proc_maps] - Print information on allocated process memory ranges
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_dump_maps mac_dump_maps] - Dumps memory ranges of processes
      * *Kernel Memory and Objects*
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_list_sessions mac_list_sessions] - Enumerates sessions
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_list_zones mac_list_zones] - Enumerates zones (allocated/freed object counts)
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_lsmod mac_lsmod] - Lists loaded kernel modules
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_mount mac_mount] - Prints mounted device information
      * *Networking*
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_arp mac_arp] - Prints the arp table
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_ifconfig mac_ifconfig] - Lists network interface information for all devices
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_netstat mac_netstat] - Lists active per-process network connections
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_route mac_route] - Prints the routing table
      * *Malware/Rootkits*
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_check_sysctl mac_check_sysctl] - Check for unknown sysctl handlers
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_check_syscalls mac_check_syscalls] - Check for hooked syscall table entries
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_check_trap_table mac_check_trap_table] - Checks to see if mach trap table entries are hooked
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_ip_filters mac_ip_filters] - Reports any hooked IP filters
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_notifiers mac_notifiers] - Detects rootkits that add hooks into I/O Kit (e.g. !LogKext)
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_trustedbsd mac_trustedbsd] - List malicious trustedbsd policies
      * *System Information*
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_dmesg mac_dmesg] - Prints the kernel debug buffers
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_find_aslr_shift mac_find_aslr_shift] - Find the ASLR shift value for 10.8+ images
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_machine_info mac_machine_info] - Prints machine information about the sample
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_version mac_version] - Prints the Mac version 
         * <font color='red'>(new)</font> [MacCommand-Reference23#mac_print_boot_cmdline mac_print_boot_cmdline] - Prints the mac boot command line
      * *Miscellaneous*
          * <font color='red'>(new)</font> [MacCommand-Reference23#mac_volshell mac_volshell] - Shell to interactively explore mac memory captures
          * <font color='red'>(new)</font> [MacCommand-Reference23#machoinfo machoinfo] - Display header and memory runs for Mach-O memory dumps
          * <font color='red'>(new)</font> [MacCommand-Reference23#mac_yarascan mac_yarascan] - Scan for Yara signatures in process or kernel memory

# Credits  

In alphabetical order:

 * Cem Gurkok for his work on the privileges plugin for Windows
 * Nir Izraeli for his work on the VMware snapshot address space (see also the [vmsnparser](http://code.google.com/p/vmsnparser/) project)
 * @osxmem of the [volafox project](https://code.google.com/p/volafox/) (Mac OS X & BSD Memory Analysis Toolkit)
 * @osxreverser of [reverse.put.as](http://reverse.put.as/) for his help with OSX memory analysis
 * Carl Pulley for numerous bug reports, example patches, and plugin testing 
 * Andreas Schuster for his work on poison ivy plugins for Windows 
 * Joe Sylve for his work on the ARM address space and significant contributions to linux and mac  capabilities
 * Philippe Teuwen for his work on the virtual box address space 
 * Santiago Vicente for his work on the citadel plugins for Windows
