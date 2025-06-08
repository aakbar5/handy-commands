
<!-- TOC depthto:2 -->

- [Linux](#linux)
    - [Compile](#compile)
    - [Device Tree](#device-tree)
    - [GCC commands](#gcc-commands)
    - [readelf](#readelf)
    - [nm](#nm)
    - [string](#string)
    - [objdump](#objdump)
    - [strip](#strip)
    - [Misc compiler commands](#misc-compiler-commands)
    - [Misc](#misc)
- [Yocto](#yocto)
    - [Recipes](#recipes)
    - [bbappend](#bbappend)
    - [Layers](#layers)
    - [Images](#images)
    - [Misc commands](#misc-commands)
- [Buildroot](#buildroot)
- [Misc](#misc)
    - [Setup TFTP](#setup-tftp)

<!-- /TOC -->

# Linux

## Compile
- `make ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu- clean` - Remove object files
- `make ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu- distclean` - Remove intermediate files including .config
- `make ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu- mrproper` - 
- `make ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu- <defconfig>`
- `make ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu- menuconfig`
- `make ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu- savedefconfig`
- `make ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu- zImage`
- `make ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu- <dt filename>.dtb` - Build an individual dts file
- `make ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu- dtbs- ` - Build all DTS
- `make ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu- modules- `
- `sudo make ARCH=arm64 INSTALL_MOD_PATH=<path to root of file system> modules_install`
 - `INSTALL_MOD_STRIP=1` - Pass it on to strip down debug info from the kernel module


## Device Tree
- [Convert DTS to DTB](https://gist.github.com/aakbar5/60e432b4e16843bef8656de88ab1e1b7)
- [Convert DTB to DTS](https://gist.github.com/aakbar5/60e432b4e16843bef8656de88ab1e1b7)


## GCC commands
- Add `-P -E` to gcc commandline to generate pre-process code for inspection
- Add `-S` to gcc commandline to generate assembly code
- `gcc -dM -E - < /dev/null` - See list of compiler default macros in standard build
- `gcc -dM -E -O0 - < /dev/null` - See list of the compiler default macros with optimization level 0
- `g++ -dM -E -x c++ -std=c++11 - < /dev/null` - Compiler default macros in case of c++11
- `gcc --help=warnings` - Compiler options to control warning
- `gcc -Q --help=warning | sed -e 's/^\s*\(\-\S*\)\s*\[\w*\]/\1 /gp;d' | tr -d '\n'` - Make one long string of the compiler options
- `gcc -Q --help=warning` - Show enable/disable state of compiler warning flags
- `gcc -Wall -Wextra -Q --help=warning` - Show enable/disable state of compiler warning flags on using `-Wall -Wextra`


## readelf
- `readelf --relocs compilation_example.o` - Relocation symbols as shown by readelf
- `readelf --sections --wide a.out` - A listing of sections in the example binary
- `readelf --syms a.out` - Symbols in the a.out binary as shown by readelf
- `readelf -h a.out` - Executable header as shown by readelf
- `readelf -h elf_header` - The readelf output for the extracted ELF header
- `readelf -p .interp a.out` - Contents of the .interp section
- `readelf -s libhello.so` - Read constants string
- `readelf --wide --syms </path/to/.so/file/or/executable/file>` - See all symbols of a .so file
- `readelf --wide --sections </path/to/.so/file/or/executable/file>` - See summary of all sections
- `readelf --file-header </path/to/.so/file/or/executable/file>` - Show header of the ELF file.
    -- for so file: TYPE is DYN (Shared object file)

- `readelf libcl.so -W -d | grep NEEDED` - Get list of the external libraries required by the ELF object
- `readelf --all app` -
- `readelf --dynamic app` - Show dynamic sections of the app
- `readelf --header app` - Show the header of the file
- `readelf --headers app | grep 'Entry point'` - Show entry point address
- `readelf --relocs app` - Show relocation sections of the app
- `readelf --segments app` - Show segment headers

## nm
- `nm -gDC libhello.so > libhello.symbols` - Extract symbols from the library
- `nm -D libhello.so | c++filt` - Demangles C++ symbols
- `nm -D --demangle lib5ae9b7f.so` - Demangled nm output for lib5ae9b7f.so
- `nm -S --size-sort -s </path/to/executable/file>` - Getting symbols
- `nm -D --demangle lib.so` - Use `nm` to demangle library symnbol

## string
- `strings -tf /bin/ls`
- `strings -d test`
- `strings -a test`
- `strings -t d test`
- `strings -a -t d libhello.so | c++filt`

## objdump
- `objdump -dM intel /path/to/executable/file` - (`-d` - Disassebmle executable sections), (`-M` - Intel specific syntax)
- `objdump -DM intel /path/to/executable/file` - Disassemble all
- `objdump -f /path/to/executable/file` - Display file headers
- `objdump -g /path/to/executable/file` - Display debug information
- `objdump -h /path/to/executable/file` - Display section headers
- `objdump -M intel -d a.out` - Disassembling an executable with objdump
- `objdump -p /path/to/executable/file` - Show information that is specific to the object file format
- `objdump -R /path/to/executable/file` - Display the dynamic relocation table
- `objdump -s /path/to/executable/file` - Display full contents of any sections
- `objdump -sj .rodata compilation_example.o` - Disassembling an object file (Show .rodata section only)
- `objdump -T --demangle /path/to/executable/file` - Demangle symbol table
- `objdump -T /path/to/executable/file` - Display the dynamic symbol table
- `objdump -t /path/to/executable/file` - Display the symbol table
- `objdump -x /path/to/executable/file` - Display all headers
- `objdump ls.ctor -s --section=.init_array`
- `objdump -p /path/to/program | grep NEEDED` - Get list of the external libraries required by the ELF object
- `objdump --disassemble ./test | grep 100002b0`
- `objdump --disassemble-all ./hello`
- `objdump --syms ./fuse.ko | grep modinfo`
- `objdump -T lib.so` - View exported symbols
- `objdump -T --demangle lib.so` - View export symbols and demangle those

## strip
- `strip --strip-all a.out` - Stripping an executable
- `strip --strip-debug </path/to/input/lib/program> -o </path/to/output/file>` - Strip debug info

## Misc compiler commands
- `make kernelversion` - To see kernel version.
- `size --common --totals <path/to/obj/file/or/path/to/executable>` - Show sizes of the each section
    ```
    aarch64-poky-linux-size --common --totals vmlinux
    text	   data	    bss	    dec	    hex	filename
    15036748	8039596	 441152	23517496	166d938	vmlinux
    15036748	8039596	 441152	23517496	166d938	(TOTALS)
    ```
- `rm /etc/ld.so.cache && ldconfig` - Force the system to rebuild ld cache
- `addr2line -f -e vmlinux 0xDEADBEAF` - Map address onto line in the source code
- `<linux/source>/scripts/faddr2line vmlinux native_write_msr+0x6` - kernel secript to translate a stack dump function
- `hexdump -C /bin/ls`
- `LD_DEBUG=all /path/to/app` - Can be used to dump debug info generated by the Linux dynamic linker

    ```
    Valid options for the LD_DEBUG environment variable are:

    libs        display library search paths
    reloc       display relocation processing
    files       display progress for input file
    symbols     display symbol table processing
    bindings    display information about symbol binding
    versions    display version dependencies
    all         all previous options combined
    statistics  display relocation statistics
    unused      determined unused DSOs
    help        display this help message and exit
    ```
    Ref: http://schillix.sourceforge.net/man/man1/ld.so.1.1.html


## Misc

- `udevadm control --reload-rules && udevadm trigger` - For udev to reload rules
    - https://marc.info/?l=linux-usb&m=121459435621262&w=2

- Verify config of the running kernel
	- Dump config: `cat /proc/config.gz | gunzip > active_config.config` or `zcat /proc/config.gz > active_config.config`
    - Needs kernel support for this: `General setup > Kernel .config support > Enable access to .config through /proc/config.gz`.

- DRM manipulation via commandline
    - Chanage debug level
        - `cat /sys/module/drm/parameters/debug`
        - `echo 0xff > /sys/module/drm/parameters/debug`
           - `drm.debug=0x1` will enable `CORE` messages
           - `drm.debug=0x2` will enable `DRIVER` messages
           - `drm.debug=0x3` will enable `CORE` and `DRIVER` messages
           - ...
           - `drm.debug=0x1ff` will enable all messages

    - Device status
        - `cat /sys/class/drm/card0-HDMI-A-1/status`
        - `cat /proc/meminfo | grep -i cma`
        - `cat /sys/kernel/debug/dma_buf/bufinfo`
        - `cat /sys/kernel/debug/dri/0/state`

# Yocto
## Recipes

- `bitbake <package-name_OR_recipe_name> -c listtasks` - Show tasks associated for a specific package/recipe.
- `bitbake <package-name_OR_recipe_name> -f -c <task_name>` - `-f` is forcing bitbake to perform the task. `<task_name>` Retrieved from the above command without `do_`.
- `bitbake <package-name_OR_recipe_name> -c devshell` - Open a new shell where build env is setup for a specific package/recipe.
- `bitbake -s | grep <package-name_OR_recipe_name>` - Check if certain package is present on current Yocto Setup
- `bitbake -e <recipe_name> | grep ^PV` - To find out recipe version which will be used to build image
	- To override selection, use `PREFERRED_VERSION_<recipe> = <version>` in your distro or local configuration.

- `bitbake virtual/kernel -c compile` - Build a kernel
- `bitbake virtual/kernel -c menuconfig` - Show menuconfig for the kernel
- `bitbake virtual/kernel -c diffconfig` - Show diff in config
- `bitbake virtual/kernel -c cleanall` - Clean the kernel build

- `bitbake -e virtual/kernel | grep "^PN"` - To find out package providing kernel
- `bitbake -e u-boot | grep "^WORKDIR"` - To find out folder used as working directory

## bbappend

- `bbappend` files can be used to pass-on new configuration to a recipe.

### Enable new feature
- Add features using `bbappend`

Enable features:
```bash
PACKAGECONFIG ?= "feature1"
PACKAGECONFIG += "feature2"
```

Pass different options as per new features:
```bash
PACKAGECONFIG[feature1] = "--enable-feature1,--disable-feature1,dependencies"
PACKAGECONFIG[feature2] = "--enable-feature2,--disable-feature2,dependencies
```

### Pass build flags

- Pass new flags to build:
```bash
EXTRA_OECMAKE += " -DENABLE_MODULE_1=1 -DEXTRA_FLAGS=1"
```

## Layers

- `bitbake-layers show-layers` - Show currently configured layers
- `bitbake-layers show-recipes` - Show all recipes and their versions alongwith the layer
- `bitbake-layers show-recipes <recipe-name>` - Show all recipes and their versions for a specific recipe
		- `PREFERRED_VERSION_<recipe-name>` - To use a specific version.

- `bitbake-layers show-recipes "*-image-*"` - Show recipes to produce image(s).
- `bitbake-layers show-appends ` - Show all .bbappend files and their respective recipes
- `bitbake-layers show-overlayed ` - Show overlayed recipes

## Images

- `bitbake -e <image-name> | grep '^DISTRO_FEATURES'` - Grep value `DISTRO_FEATURES` used to build <image-name>
- `bitbake -e <image-name> | grep '^IMAGE_FSTYPES='` - Grep value `DISTRO_FEATURES` used to build <image-name>
- `bitbake -g <image-name>` - It will produce a couple of files which can be used to extract info related to pulled packages/versions.
- `bitbake -g <image-name> -u depexp` - Show package dependency for <image-name>
- `bitbake -g <image-name> && cat pn-buildlist | sort | uniq` - Show all packages pulled for the <image-name>
- `bitbake -g <image-name> && cat pn-buildlist | grep -ve "native" | sort | uniq` - Show all packages pulled for <image-name> especially for `native`

## Misc commands

### Find out toolchain used by the Yocto
`bitbake -e | grep EXTERNAL_TOOLCHAIN_SYSROOT=`

### Expand disk size
- Create an extra disk space in rootfs.
- Following commands gives 3.7GB of rootfs to fit in a 4GB sdcard.
```
IMAGE_EXTRA_SPACE = "0"
IMAGE_ROOTFS_EXTRA_SPACE = "0"
IMAGE_ROOTFS_SIZE = "3806250"
```

# Buildroot
- `make <package-name>-dirclean` - Clean build of the package # `<package-name>`
- `make <package-name>-rebuild` - Rebuild the package # `<package-name>`
- `make <package-name>-reconfigure` - Reconfigure the package # `<package-name>`
- `make <package-name>-reinstall` - Reinstall the package # `<package-name>` into the rootfs
- `make` - Build all packages and generate rootfs

- `make linux-dirclean` - Clean build folder of the kernel
- `make linux-rebuild` - Rebuild kernel
- `make linux-menuconfig` - Run menuconfiq of the kernel
- `make savedefconfig` - Save config of buildroot (`cp defconfig arch/arm/configs/my_cool_defconfig`)
    - `make savedefconfig BR2_DEFCONFIG=<path-to-defconfig>`
    
- `rm -rf </path/to/buildroot/folder>/output/build/linux-*` - Manually remove build artifact related to the kernel
- `rm </path/to/buildroot/folder>/dl/linux-*` - Manually remove download files related to the kernel
- Config files
    ```text
    linux-savedefconfig for linux
    barebox-savedefconfig for barebox bootloader
    uboot-savedefconfig for U-Boot bootloader
    ```

# Misc
## Setup TFTP
```bash
sudo apt install -y atftpd
sudo sed -i 's/USE_INETD=true/USE_INETD=false/' /etc/default/atftpd
sudo systemctl start atftpd
sudo systemctl enable atftpd
```

### Setup NFS
```bash
sudo apt install -y nfs-kernel-server
sudo mkdir -p /srv/nfs
sudo chmod -R 777 /srv/nfs

sudo service nfs-kernel-server status
```
- edit `/etc/exports`
```bash
echo '/srv/nfs 192.168.0.200/24(rw,sync,no_subtree_check,no_root_squash)' | sudo tee -a /etc/exports
echo '/srv/nfs *(rw,sync,no_subtree_check,no_root_squash)' | sudo tee -a /etc/exports
```

- Reload stuff
```bash
sudo exportfs -ra
```

- View NFS
```bash
sudo exportfs -v
sudo rpcinfo -p
```

- To test
```bash
nfs://[hostname-or-ip-of-pi]/srv/nfs
```
