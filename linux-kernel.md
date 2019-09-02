# Table of Contents
- [Kernel](#kernel)
- [Device Tree](#dtc)
- [GCC](#gcc)
- [Misc](#misc)

<a name="kernel"></a>
## Kernel
- `make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- clean` - Remove object files
- `make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- mrproper` - Remove intermediate files including .config
- `make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- distclean`
- `make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- <defconfig>`
- `make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- menuconfig`
- `make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- zImage`
- `make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- <dt filename>.dtb` - Build an individual dts file
- `make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- dtbs- ` - Build all DTS
- `make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- modules- `
- `sudo make ARCH=arm INSTALL_MOD_PATH=<path to root of file system> modules_install`
 - `INSTALL_MOD_STRIP=1` - Pass it on to strip down debug info from the kernel module

<a name="dtc"></a>
## Device Tree
- [Convert DTS to DTB](https://gist.github.com/aakbar5/60e432b4e16843bef8656de88ab1e1b7)
- [Convert DTB to DTS](https://gist.github.com/aakbar5/60e432b4e16843bef8656de88ab1e1b7)

<a name="gcc"></a>
## GCC
- Add `-P -E` to gcc commandline to generate pre-process code for inspection
- Add `-S` to gcc commandline to generate assembly code
- `gcc -dM -E - < /dev/null` - See list of compiler default macros in standard build
- `gcc -dM -E -O0 - < /dev/null` - See list of the compiler default macros with optimization level 0
- `g++ -dM -E -x c++ -std=c++11 - < /dev/null` - Compiler default macros in case of c++11


<a name="misc"></a>
## Misc

- `make kernelversion` - To see kernel version.
- `size --common --totals <path/to/obj/file/or/path/to/executable>` - Show sizes of the each section
    ```
    aarch64-poky-linux-size --common --totals vmlinux
    text	   data	    bss	    dec	    hex	filename
    15036748	8039596	 441152	23517496	166d938	vmlinux
    15036748	8039596	 441152	23517496	166d938	(TOTALS)
    ```
- `readelf --wide --syms </path/to/.so/file/or/executable/file>` - See all symbols of a .so file
- `readelf --wide --sections </path/to/.so/file/or/executable/file>` - See summary of all sections
- `readelf --file-header </path/to/.so/file/or/executable/file>` - Show header of the ELF file.
                                            -- for so file: TYPE is DYN (Shared object file)

- `nm -S --size-sort -s </path/to/executable/file>` - Getting symbols
- `nm </path/to/c++/library> | c++filt` - Demangles C++ symbols
- `rm /etc/ld.so.cache && ldconfig` - Force the system to rebuild ld cache
- `readelf libcl.so -W -d | grep NEEDED` - Get list of the external libraries required by the ELF object
- `objdump -p /path/to/program | grep NEEDED` - Get list of the external libraries required by the ELF object
