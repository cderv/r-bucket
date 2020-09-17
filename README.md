![](https://github.com/cderv/r-bucket/workflows/Excavator/badge.svg)

# Scoop bucket for an R user

This repos contains some manifests I use to quickly install and update some applications useful for an R user that I don't find in any other bucket.

## Apps in this bucket

### RStudio IDE 

* RStudio 1.2 (installer-less) 
* RStudio daily (installer-less) - Synced from https://dailies.rstudio.com/

### TinyTeX - TexLive distribution (Experimental - this could change)

3 binaries are available from [tinytex-releases](https://github.com/yihui/tinytex-releases) but only one can be installed at the same time. That is because a _shim_ for `tlmgr` command is added to _scoop_ and available to PATH, and that would be overriden. (And honestly, you only need one). 

* `tinytex` contains [several tex packages already installed](https://github.com/yihui/tinytex/blob/master/tools/pkgs-custom.txt) that are the main one used by Rmarkdown. If you are an R user we recommand this one. 
* `tinytex-min` is infra-only and contains only Tex Live with no package installed. Use this one if you want only Tex Live for Windows using [TinyTeX distribution](https://yihui.org/tinytex/) without the selected packages, for CI workflow for example. 
* `tinytex-full` contains more packages than `TinyTeX`.

See full documentation at https://yihui.org/tinytex/ and about the releases at https://github.com/yihui/tinytex-releases#scoop-package

The binaries are synced from https://github.com/yihui/tinytex-releases/releases

## To install scoop 

See https://github.com/lukesampson/scoop

## Add this scoop bucket

```powershel
scoop bucket add r-bucket https://github.com/cderv/r-bucket.git
```

## Install an app

```powershell
scoop install rstudio-daily
scoop install TinyTeX
```

## List installed apps

```powershell
scoop list
```

## Update an app

```powershell
scoop update
scoop status
scoop update rstudio-daily
```

## Uninstall an app

```powershell
scoop uninstall rstudio-daily
```

## check if a tools is found in PATH 

```powershell
scoop which tlmgr
```
