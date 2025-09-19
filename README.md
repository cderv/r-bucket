![](https://github.com/cderv/r-bucket/workflows/Excavator/badge.svg)
# Scoop bucket for an R user

This repos contains some manifests I use to quickly install and update some applications useful for an R user that I don't find in any other bucket.

- [Apps in this bucket](#apps-in-this-bucket)
  - [RStudio IDE](#rstudio-ide)
    - [Daily versions](#daily-versions)
    - [Preview versions](#preview-versions)
    - [Released versions](#released-versions)
    - [Branched versions](#branched-versions)
  - [Positron](#positron)
    - [Site-available versions](#site-available-versions)
    - [Prereleases (Monthly build)](#prereleases-monthly-build)
    - [Dailies (Nightly build)](#dailies-nightly-build)
  - [TinyTeX - Tex Live distribution](#tinytex---tex-live-distribution)
  - [R](#r)
    - [R devel](#r-devel)
    - [rig - R Installation Manager](#rig---r-installation-manager)
    - [R release and old releases](#r-release-and-old-releases)
    - [rv - new R package installer](#rv---new-r-package-installer)
  - [Quarto CLI](#quarto-cli)
    - [Stable version](#stable-version)
    - [Pre-release version](#pre-release-version)
    - [Switching between stable and pre-release](#switching-between-stable-and-pre-release)
  - [Quarto Versions Manager - QVM](#quarto-versions-manager---qvm)
    - [Should I install `quarto` using `qvm` or install `quarto` with scoop directly ?](#should-i-install-quarto-using-qvm-or-install-quarto-with-scoop-directly-)
  - [Sass migrator](#sass-migrator)
  - [air](#air)
  - [Ark](#ark)
  - [Chrome Headless Shell](#chrome-headless-shell)
- [To install scoop](#to-install-scoop)
- [Add this scoop bucket](#add-this-scoop-bucket)
- [Install an app](#install-an-app)
- [List installed apps](#list-installed-apps)
- [Update an app](#update-an-app)
- [Uninstall an app](#uninstall-an-app)
- [check if a tools is found in PATH](#check-if-a-tools-is-found-in-path)

## Apps in this bucket

Below is a list of the app available through this bucket. This can also be found in [scoop.sh Apps listing](https://scoop.sh/#/apps?q=%22https%3A%2F%2Fgithub.com%2Fcderv%2Fr-bucket%22&o=false). 

### RStudio IDE 

#### Daily versions

* `rstudio-daily`: RStudio daily (installer-less) - Synced from https://dailies.rstudio.com/
* `rstudio-pro-daily`: RStudio Pro daily (installer-less) - Synced from https://dailies.rstudio.com/

#### Preview versions

* `rstudio-preview`: RStudio Preview (installer-less) - Synced from https://rstudio.com/products/rstudio/download/preview/
* `rstudio-pro-preview`: RStudio Pro Preview (installer-less) - Synced from https://rstudio.com/products/rstudio/download/preview/

#### Released versions

* `rstudio-release`: RStudio released version (installer-less) - Synced from https://rstudio.com/products/rstudio/download/
* `rstudio-pro-release`: RStudio Pro release version (installer-less) - Synced from https://rstudio.com/products/rstudio/download/

#### Branched versions

Only available in this bucket on the Open Source version. 
Synced from https://dailies.rstudio.com/rstudio/

* `rstudio-1.2`: RStudio 1.2 (installer-less) - Freezed to last available 1.2 version
* `rstudio-1.3`: RStudio 1.3 (installer-less) - Freezed to last available 1.3 version
* `rstudio-1.4`: RStudio 1.3 (installer-less) - Freezed to last available 1.4 version (Ghost Orchid)
* `rstudio-2022.02`: RStudio 2022.02 (installer-less) - Last available version for Prairie Trillium branch
* `rstudio-2022.07`: RStudio 2022.06 (installer-less) - Last available version for Spotted Wakerobin branch
* `rstudio-2022.12`: RStudio 2022.12 (installer-less) - Last available version for Elsbeth Geranium branch
* `rstudio-2023.03`: RStudio 2023.03 (installer-less) - Last available version for Cherry Blossom branch
* `rstudio-2023.06`: RStudio 2023.06 (installer-less) - Last available version for Mountain Hydrangea branch
* `rstudio-2023.09`: RStudio 2023.09 (installer-less) - Last available version for Desert Sunflower branch
* `rstudio-2023.12`: RStudio 2023.12 (installer-less) - Last available version for Ocean Storm branch
* `rstudio-2024.04`: RStudio 2024.04 (installer-less) - Last available version for Desert Chocolate Cosmos branch


### Positron 

Positron (<https://github.com/posit-dev/positron>) is :

- a next-generation data science IDE built by Posit PBC
- An extensible, polyglot tool for writing code and exploring data
- A familiar environment for reproducible authoring and publishing

> [!IMPORTANT]
> Positron is an early stage project under active development and may [not yet be a good fit for you](https://github.com/posit-dev/positron/wiki#is-positron-for-me) !

#### Site-available versions

This is the version offered to be downloaded at <https://positron.posit.co/download.html>. It will be available in PATH as `positron`.

```powershell

````powershell
# using r-bucket as in `scoop bucket add r-bucket https://github.com/cderv/r-bucket.git` when installing bucket
scoop install r-bucket/positron
````

#### Prereleases (Monthly build)

This version can't be used at the same time as the other positron pre-release install above as it will be available in PATH as `positron` also.
It follows version published at `https://cdn.posit.co/positron/prereleases/` 

```powershell
scoop install positron-prerelease
```

#### Dailies (Nightly build)

This version is the build pushed daily. It can be installed and use at the same time of other positron version, as it will be available in PATH as `positron-daily`.
It follows version published at `https://cdn.posit.co/positron/dailies/`

```powershell
scoop install positron-daily
```

### TinyTeX - Tex Live distribution
> Experimental - this could still change

3 binaries are available from [tinytex-releases](https://github.com/yihui/tinytex-releases) but only one can be installed at the same time. That is because a _shim_ for `tlmgr` command is added to _scoop_ and available to PATH, and that would be overriden. (And honestly, you only need one). 

* `tinytex` contains [several tex packages already installed](https://github.com/yihui/tinytex/blob/master/tools/pkgs-custom.txt) that are the main one used by Rmarkdown. If you are an R user we recommand this one. 
* `tinytex-min` is infra-only and contains only Tex Live with no package installed. Use this one if you want only Tex Live for Windows using [TinyTeX distribution](https://yihui.org/tinytex/) without the selected packages, for CI workflow for example. 
* `tinytex-extra` contains some extra packages installed compare to `tinytex`.

See full documentation at https://yihui.org/tinytex/ and about the releases at https://github.com/yihui/tinytex-releases#scoop-package

The binaries are synced from https://github.com/yihui/tinytex-releases/releases

### R 

#### R devel

You can install R devel for Windows using this bucket containing the `r-devel` app. The version will be synced to https://cran.r-project.org/bin/windows/base/rdevel.html and you will always have the last available r-devel version, tracked by the version notation like r79818 for the current development snapshot of R. As any other app, a new version will be checked for daily. 

This scoop installed app will behave like with the apps in the _Versions_ bucket (https://github.com/ScoopInstaller/Versions): You can have several versions of R installed with scoop but only one in use. You will have to use [`scoop reset`](https://scoop-docs.now.sh/docs/misc/Switching-Ruby-and-Python-Versions.html#scoop-reset) to switch between installed versions.

```powershell
# from this bucket
scoop install r-devel
rterm --version # r-devel

# from the main bucket
scoop install r-release
rterm --version # r current release

# switching back to use r-devel
scoop reset r-devel
rterm --version # r-devel
```

#### rig - R Installation Manager

rig, the R Installation Manager, (https://github.com/r-lib/rig) is a tool to install, remove, configure R versions. 

This bucket contains the manifest to install and update easily the [rig CLI](https://github.com/r-lib/rig/releases) for Windows. 

```powershell
# install
scoop install rig

# update
scoop update rig
```

Using `rig`, you can then install and manage any R versions. See `rig help` for details.

#### R release and old releases

If you prefer installing R versions with scoop, you can can 

- install R current release.

```powershell
scoop install r-release
```

- or install a previous release

```powershell
scoop install r-release@3.5.2
```

You can then switch between R versions (including r-devel - see below) using 

```powershell
scoop reset r-release
scoop reset r-release@3.5.2
```

[R Installation Manager (RIM)](#r-installation-manager-rim), installable from this bucket too, is a better tool for installing R versions.

#### rv - new R package installer

rv is a new way to manage and install your R packages in a reproducible, fast, and declarative way. See <https://github.com/A2-ai/rv>

```powershell
# install
scoop install rv

# update
scoop update rv
```

### Quarto CLI

Quarto (https://quarto.org/) is an open-source scientific and technical publishing system built on Pandoc. Quarto documents are authored using markdown, an easy to write plain text format.

This bucket contains the manifests to install and update easily the [quarto CLI](https://github.com/quarto-dev/quarto-cli/releases) for Windows for stable versions and pre-release versions.

#### Stable version

```powershell
# install
scoop install quarto

# update
scoop update quarto
```

You can also install and manage Quarto using [Quarto Versions Manager - qvm](#quarto-versions-manager---qvm)

#### Pre-release version

```powershell
scoop install quarto-prerelease

scoop update quarto-prerelease
```

#### Switching between stable and pre-release

`quarto` and `quarto-prerelease` will both make `quarto` binary available in PATH. Only one version can be used at the same time. To switch between version use the following: 

```powershell
scoop install quarto
quarto --version # e.g 1.0.35

# installing second
scoop install quarto-prerelease
quarto --version # e.g 1.1.3

# Switching back to stable
scoop reset quarto
quarto --version # e.g 1.0.35

# Switching again to pre-release
scoop reset quarto-prerelease
quarto --version # e.g 1.1.3
```

### Quarto Versions Manager - QVM

[Quarto Version Manager](https://github.com/dpastoor/qvm) is a CLI tools that allows to manage and switch between versions of Quarto easily. 

This bucket contains the manifest to install and update easily the [qvm releases](https://github.com/dpastoor/qvm/releases) for Windows.

#### Should I install `quarto` using `qvm` or install `quarto` with scoop directly ?

QVM is aimed toward developers using Quarto that quickly needs to switch between versions. For only installing quarto and staying up to date, install [quarto](#quarto-cli) directly from this bucket will be enough. **It is not recommended to have both installed**.

```powershell
# install
scoop install qvm

# update
scoop update qvm
```

### Sass migrator

[Sass Migrator](https://github.com/sass/migrator) is a CLI tool to help migrate old to new syntax.

The Sass migrator automatically updates stylesheets that use deprecated behavior to the latest and greatest Sass features. 

See documentation on main website: https://sass-lang.com/documentation/cli/migrator/

```powershell
# install
scoop install sass-migrator

# update
scoop install sass-migrator
```

### air

[air](https://github.com/posit-dev/air) is an R formatter and language server, written in Rust.  
**air is currently in alpha. Expect breaking changes both in the API and in formatting results.** 

As `air` is also a tool available in other scoop bucket, it needs to be namespaced to be installed

```powershell
# install 
# using r-bucket as in `scoop bucket add r-bucket https://github.com/cderv/r-bucket.git` when installing bucket
scoop install r-bucket/air

# update
scoop install air
```


### Ark

[Ark](https://github.com/posit-dev/ark) is an R kernel for Jupyter applications. It provides an R-backed kernel to run R code inside Jupyter notebooks and other Jupyter frontends. See the upstream project for full details and usage instructions.

```powershell
# install 
# using r-bucket as in `scoop bucket add r-bucket https://github.com/cderv/r-bucket.git` when installing bucket
scoop install r-bucket/ark

# update
scoop update ark
```

### Chrome Headless Shell

`chrome-headless-shell` is a new binary based on old chrome headless mode, dedicated to screenshoting, pdf printing or web-scraping. See more at <https://developer.chrome.com/blog/chrome-headless-shell>.

```powershell
# install 
# using r-bucket as in `scoop bucket add r-bucket https://github.com/cderv/r-bucket.git` when installing bucket
scoop install r-bucket/chrome-headless-shell

# update
scoop install chrome-headless-shell
````

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

You can also namespace app in this bucket if there is a conflict

```powershell	
# using r-bucket as in `scoop bucket add r-bucket https://github.com/cderv/r-bucket.git` when installing bucket
scoop install r-bucket/air
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
