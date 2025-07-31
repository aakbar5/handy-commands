# Uboot

- [General commands](#general)
- [Memory](#memory)
- [File System](#file_system)
- [MMC](#mmc)
- [USB](#usb)
- [Network](#network)
- [NAND Flash](#nand_flash)
- [Environment Variables Commands](#env_variables)

<a name="general"></a>
## General commands

- `bdinfo` - print Board Info structure
- `coninfo` - print console devices and information
- `flinfo` - print FLASH memory information
- `iminfo addr` - Print header information for application image
- `crc address count [save_addr]` - calculates CRC32
- `echo` - echo arguments to console
- `reset` - Perform RESET of the CPU
- `sleep` - delay execution for some time
- `version` - print monitor version
- `test` - minimal test, like /bin/bash
- `autoscr` - Run script from memory
- `bootm` - Boot application image from memory
- `go` - Start application at address addr

<a name="memory"></a>
## Memory
- `cmp [.b, .w, .l] addr1 addr2 count`
The cmp command tests of the contents of two memory areas and determines whether or not the contents of the two memory areas are identical or not. The command will either test the whole area as specified by the 3rd (count) argument or stop at the first difference if the count argument is not specified.

- `cp [.b, .w, .l] source target count`
memory copy

- `md [.b, .w, .l] address [# of objects]`
memory display

- `mm [.b, .w, .l] address`
memory modify (auto-incrementing address)
The mm is a method to interactively modify memory contents. It will display the address and current contents and then prompt for user input. If you enter a legal hexadecimal number, this new value will be written to the address. Then the next address will be prompted. If you don't enter any value and just press Enter, then the contents of this address will remain unchanged. The command stops as soon as you enter any data that is not a hex number:

- `mtest [start [end [pattern [iterations]]]]`
simple RAM read/write test
This tests writes to memory, thus modifying the memory contents. It will fail when applied to ROM or
flash memory. This command may crash the system when the tested memory range includes areas that are
needed for the operation of the U-Boot firmware (like exception vector code, or U-Boot's internal program
code, stack or heap memory areas).

- `mw [.b, .w, .l] address value [count]`
memory write (fill)

- `nm [.b, .w, .l] address`
memory modify (constant address)

- `loop [.b, .w, .l] address number_of_objects`
infinite loop on address range

- `base` - Print or set address offset

<a name="file_system"></a>
## File System
- `fatinfo` - Print info about a FAT FS
- `fatload` - Load binary file from a FAT  FS
- `fatls` - List files in a FAT FS
- `fsinfo` - Print info about JFFS2 FS
- `fsload` - Load bin file from JFFS2FS
- `ls` - List file in a JFFS@ FS

<a name="mmc"></a>
## MMC
MMC
- todo

<a name="usb"></a>
## USB
- todo

<a name="network"></a>
## Network
- todo

<a name="cache"></a>
## Cache
- todo

<a name="nand_flash"></a>
## NAND Flash
- `nand` - NAND sub-system control
- `nboot` - Boot from NAND Device
- `cp` - Memory copy
- `flinfo` - Print flash memory information
- `erase` - Erase flash memory
- `protect` - Enable or disable flash write protection

<a name="env_variables"></a>
## Environment Variables Commands
- `setenv var value` - Set/update a variable
- `setenv var` - Delete a variable
- `printenv` - Print env variables
- `saveenv` - Save env variables to storage
- `run` - Run commands in an env variable
- `bootd` - Boot default
