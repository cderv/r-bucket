# Scoop bucket for an R user

This repos contains some manifests I use to quickly install and update some applications useful for an R user that I don't find in any other bucket.

## Apps in this bucket

* RStudio 1.2 (installer-less)
* RStudio daily (installer-less)

## To install scoop 

See https://github.com/lukesampson/scoop

## Add this scoop bucket

```powershel
scoop bucket add r-bucket https://github.com/cderv/r-bucket.git
```

## Install an app

```powershell
scoop install rstudio-daily
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