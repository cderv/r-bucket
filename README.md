![](https://github.com/cderv/r-bucket/workflows/Excavator/badge.svg)

# Scoop bucket for an R user

This repos contains some manifests I use to quickly install and update some applications useful for an R user that I don't find in any other bucket.

## Apps in this bucket

### RStudio IDE 

* RStudio 1.2 (installer-less) 
* RStudio daily (installer-less) - Synced from https://dailies.rstudio.com/

### TinyTex - TexLive distribution (Experimental - this could change)

3 binaries are available from [tinytex-releases](https://github.com/yihui/tinytex-releases) but only one can be installed at the same time. That is because a _shim_ for `tlmgr` command is added to _scoop_ and available to PATH, and that would be overriden. (And honestly, you only need one). 

* `TinyTex` contains several tex packages already installed that are the main one used by Rmarkdown. If you are an R user we recommand this one. 
* `TinyTex-min` is infra-only and contains only TexLive with no package installed. We recommand this one if you want Texlive for Windows.
* `TinyTex-full` contains more packages than `TinyTex`.

See full documentation at https://yihui.org/tinytex/ and about the releases at : https://github.com/yihui/tinytex-releases#scoop-package

The binaries are synced from https://github.com/yihui/tinytex-releases/releases

### R devel 

You can install R devel for Windows using this bucket containing the `r-devel` app. The version will be synced to https://cran.r-project.org/bin/windows/base/rdevel.html and you will always have the last available r-devel version, tracked by the version notation like r79818 for the current development snapshot of R. As any other app, a new version will be checked for daily. 

This scoop installed app will behave like with the apps in the _Versions_ bucket (https://github.com/ScoopInstaller/Versions): You can have several versions of R installed with scoop but only one in use. You will have to use [`scoop reset`](https://scoop-docs.now.sh/docs/misc/Switching-Ruby-and-Python-Versions.html#scoop-reset) to switch between installed versions.

```powershell
# from this bucket
scoop install r-devel
rterm --version # r-devel

# from the main bucket
scoop install r
rterm --version # r release

# switching back to use r-devel
scoop reset r-devel
rterm --version # r-devel
```

## To install scoop 

See https://github.com/lukesampson/scoop

## Add this scoop bucket

```powershell
scoop bucket add r-bucket https://github.com/cderv/r-bucket.git
```

## Install an app

```powershell
scoop install rstudio-daily
scoop install TinyTex
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
