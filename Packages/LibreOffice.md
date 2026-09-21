# LibreOffice

To have seamless GNOME-style, LibreOffice need to be run with:
1. This package installed: `libreoffice-gtk3`
2. This environment variable: `SAL_USE_VCLPLUGIN=gtk3`

In BlankOn Sinambung, this environment variable has been set to `/etc/environment` via live-build hooks.

As for `libreoffice-gtk4`, it is known to have some bugs like:
1. Can't export Writer document to PDF
2. Some buttons in Calc can't be clicked

## Repackaging Approach

TBW.
