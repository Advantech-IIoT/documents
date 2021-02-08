
# BitBake Useful Commands

---

## Contents

- [Build Image](#Build-Image)
- [Do Package's Task](#Do-Package's-Task)

---

## Build Image
Bake an image (add -k to continue building even errors are found in the tasks execution)
```shell=
$ bitbake <image>
```


## Do Package's Task
Execute a particular package's task. Default Tasks names: fetch, unpack, patch, configure, compile, install, package, package_write, and build.
```shell=
$ bitbake <package> -c <task>	
```

Example: To (force) compiling a kernel and then build, type:
```shell=
$ bitbake linux-imx -f -c compile
$ bitbake linux-imx
```

## Package's Dependency
Show the package dependency for image.
```shell=
$ bitbake <image > -g -u depexp
```

Example: To show all packages included on fsl-image-gui
```shell=
$ bitbake fsl-image-gui -g -u depexp
```
NOTE: This command will open a UI window, so it must be execute on a 
console inside the host machine (either virtual or native).


## Package's Devshell

Open a new shell where with neccesary system alues already defined for package hob bitbake frontend/GUI.
```shell=
$ bitbake <package> -c devshell
```

## List Package's Tasks 

List all tasks for package
```shell=
$ bitbake <package> -c listtasks
```

## Kernel Coniguration

Interactive kernel configuration
```shell=
$ bitbake virtual/kernel -c menuconfig
```

## Fetch Image's Sources

Fetch sources for a particular image
```shell=
$ bitbake <image> -c fetchall
```

## Layers

Show layers
```shell=
$ bitbake-layers show-layers	
```

## Recipes

Show possible images to bake. Without "*-images-*", it shows ALL recipes
```shell=
$ bitbake-layers show-recipes "*-image-*"	
```

## Image's Packages

Show image's packages
```shell=
$ bitbake -g <image> && cat pn-depends.dot | grep -v -e '-native' | grep -v digraph | grep -v -e '-image' | awk '{print $1}' | sort | uniq
```

## Package's Dependencies

Show package's dependencies
```shell=
$ bitbake -g <pkg> && cat pn-depends.dot | grep -v -e '-native' | grep -v digraph | grep -v -e '-image' | awk '{print $1}' | sort | uniq
```

## Verbose

Print (on console) and store verbose baking
```shell=
$ bitbake –v <image> 2>&1 | tee image_build.log
```
Print all log
```shell=
$ bitbake <image> -vDDD
```

## Package List

Check if certain package is present on current Yocto Setup
```shell=
$ bitbake -s | grep <pkg>
```

## Force to Do Package's Task

force to do package's task again
```shell=
$ bitbake <package> -C <task> 
$ bitbake <package> -c <task>  -f
```

# devtool
- A tool for adding or modifying source in workspace

## Workflow

- Add recipe
- Modify existed recipe

### Modify Work Flow

- Add Source into Workspace

#### Add Source into Workspace

For NXP i.mx6 Yocto Project, use command as below to fetch source into worksapce. 
```shell=
$ devltool modify linux-imx
```

---
###### tags: `bitbake` `yocto`
