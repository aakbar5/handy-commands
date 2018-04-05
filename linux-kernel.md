# Table of Contents
- [Kernel build](#kernel)
- [DTC](#dtc)

<a name="kernel"></a>
# Kernel build
```
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- distclean
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- <defconfig>
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- menuconfig
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- zImage
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- <dt filename>.dtb [Build individual dts file]
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- dtbs [Build all DTS]
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- modules
sudo make ARCH=arm INSTALL_MOD_PATH=<path to root of file system> modules_install
 - INSTALL_MOD_STRIP=1
```

<a name="dtc"></a>
# Device Tree handling
- [Convert DTS to DTB](https://gist.github.com/aakbar5/60e432b4e16843bef8656de88ab1e1b7)
- [Convert DTB to DTS](https://gist.github.com/aakbar5/60e432b4e16843bef8656de88ab1e1b7)
