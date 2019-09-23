# Table of Contents
- [Recipe](#recipes)
- [bbappend](#bbappend)
- [Layers](#layers)
- [Images](#images)
- [Misc commands](#misc)

<a name="recipes"></a>
## Recipes

- `bitbake <package-name_OR_recipe_name> -c listtasks` - Show tasks associated for a specific package/recipe.
- `bitbake <package-name_OR_recipe_name> -f -c <task_name>` - `-f` is forcing bitbake to perform the task. `<task_name>` Retrieved from the above command without `do_`.
- `bitbake <package-name_OR_recipe_name> -c devshell` - Open a new shell where build env is setup for a specific package/recipe.
- `bitbake -s | grep <package-name_OR_recipe_name>` - Check if certain package is present on current Yocto Setup
- `bitbake -e <recipe_name> | grep ^PV` - To find out recipe version which will be used to build image
	- To override selection, use `PREFERRED_VERSION_<recipe> = <version>` in your distro or local configuration.

- `bitbake virtual/kernel -c compile` - Build a kernel
- `bitbake virtual/kernel -c menuconfig` - Show menuconfig for the kernel
- `bitbake virtual/kernel -c cleanall` - Clean the kernel build

<a name="bbappend"></a>
## bbappend

- `bbappend` files can be used to pass-on new configuration to a recipe.

### Enable new feature
- Add features using `bbappend`

Enable features:
```
PACKAGECONFIG ?= "feature1"
PACKAGECONFIG += "feature2"
```

Pass different options as per new features:
```
PACKAGECONFIG[feature1] = "--enable-feature1,--disable-feature1,dependencies"
PACKAGECONFIG[feature2] = "--enable-feature2,--disable-feature2,dependencies
```

### Pass build flags

- Pass new flags to build:
```
EXTRA_OECMAKE += " -DENABLE_MODULE_1=1 -DEXTRA_FLAGS=1"
```

<a name="layers"></a>
## Layers

- `bitbake-layers show-layers` - Show currently configured layers
- `bitbake-layers show-recipes` - Show all recipes and their versions alongwith the layer
- `bitbake-layers show-recipes "*-image-*"` - Show recipes to produce image(s).
- `bitbake-layers show-appends ` - Show all .bbappend files and their respective recipes
- `bitbake-layers show-overlayed ` - Show overlayed recipes

<a name="images"></a>
## Images

- `bitbake -e <image-name> | grep '^DISTRO_FEATURES'` - Grep value `DISTRO_FEATURES` used to build <image-name>
- `bitbake -e <image-name> | grep '^IMAGE_FSTYPES='` - Grep value `DISTRO_FEATURES` used to build <image-name>
- `bitbake -g <image-name>` - It will produce a couple of files which can be used to extract info related to pulled packages/versions.
- `bitbake -g <image-name> -u depexp` - Show package dependency for <image-name>
- `bitbake -g <image-name> && cat pn-buildlist | sort | uniq` - Show all packages pulled for the <image-name>
- `bitbake -g <image-name> && cat pn-buildlist | grep -ve "native" | sort | uniq` - Show all packages pulled for <image-name> especially for `native`

<a name="misc"></a>
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
