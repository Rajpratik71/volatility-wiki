This page documents some of the recent changes in the Windows kernel that affect memory forensics and what you can do to handle the differences gracefully. 

# Notes on best practices / requirements

* The distorm3 python module is a requirement for analyzing 64-bit Windows 8/2012 (and later) raw memory images. If you're using a standalone executable, then distorm3 is already included, however if you're using Volatility from source, then you will need to install distorm3. 
* Previously, the parameter to `--kdbg` was the virtual address of the KDBG structure. On 64-bit Windows 8/2012 (and later), you pass the virtual address of `KdCopyDataBlock`. Both addresses are displayed in the output of the `kdbgscan` plugin. 

### Development notes

* The KDBG is encrypted by default on all x64 Windows 8/2012 (and later)
* The KDBG signature/size changed 
* The DTB signature changed
* New crash dump format (memory runs are bitmaps)
* Handle table pointers are encoded on x64 
* Pool tags are different (no more protected bits for executive objects)
* VAD tree structures are `_MM_AVL_NODE` instead of `_MMADDRESS_NODE` 
* New executive object types: `IRTimer`, `WaitCompletionPacket`, `DxgkSharedResource`, `DxgkSharedSyncObject`
* New optional object header (`_OBJECT_HEADER_AUDIT_INFO`)
* `win32k.sys` PDB symbols are stripped again (affects all GUI subsystem plugins)
* There's no `_HANDLE_TABLE.HandleCount` (displayed by `pslist`)
* There's no `_LDR_DATA_TABLE_ENTRY.LoadCount` (displayed by `dlllist`)
* Assembly instructions differ in `nt!KeAddSystemServiceTable` (used by `ssdt`)
* Service record offsets changed (`svcscan`)
* Offsets for undocumented networking structures changed (used by `netscan`)
