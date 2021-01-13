![](https://github.com/cderv/r-bucket/workflows/Excavator/badge.svg)

# Scoop bucket for an R user

This repos contains some manifests I use to quickly install and update some applications useful for an R user that I don't find in any other bucket.

## Apps in this bucket

### RStudio IDE 

* `rstudio-1.2`: RStudio 1.2 (installer-less) - Freezed to last available 1.2 version
* `rstudio-daily`: RStudio daily (installer-less) - Synced from https://dailies.rstudio.com/
* `rstudio-preview`: RStudio Preview (installer-less) - Synced from https://rstudio.com/products/rstudio/download/preview/

### TinyTeX - Tex Live distribution
> Experimental - this could still change

3 binaries are available from [tinytex-releases](https://github.com/yihui/tinytex-releases) but only one can be installed at the same time. That is because a _shim_ for `tlmgr` command is added to _scoop_ and available to PATH, and that would be overriden. (And honestly, you only need one). 

* `tinytex` contains [several tex packages already installed](https://github.com/yihui/tinytex/blob/master/tools/pkgs-custom.txt) that are the main one used by Rmarkdown. If you are an R user we recommand this one. 
* `tinytex-min` is infra-only and contains only Tex Live with no package installed. Use this one if you want only Tex Live for Windows using [TinyTeX distribution](https://yihui.org/tinytex/) without the selected packages, for CI workflow for example. 
* `tinytex-extra` contains some extra packages installed compare to `tinytex`.

See full documentation at https://yihui.org/tinytex/ and about the releases at https://github.com/yihui/tinytex-releases#scoop-package

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
scoop install TinyTeX
```

## List installed apps

```powershell
scoop list
```

## Update an app

```powershell
# fetch update info
scoop update
# see outdated apps
scoop status
# update one app
scoop update rstudio-daily
# update all 
scoop update *
```

## Uninstall an app

```powershell
scoop uninstall rstudio-daily
```

## check if a tools is found in PATH 

```powershell
scoop which tlmgr
```
