# Testing Package Updates

1. Precondition: you have a fresh installed system of BlankOn Sinambung's latest stable release.
2. Update the repository index
```
sudo apt update
```
3. Update the available package updates
```
sudo apt full-upgrade
```
4. Install the non-preexisting updated packages. To get the list of package,
  - Go to https://packages.blankonlinux.id/
  - Click `arsip.blankonlinux.id` tab
  - Click `Diff against arsip-dev`
  - Inspect the package list then scroll to bottom and click `Copy upgrade command`
  - Run the upgrade command

