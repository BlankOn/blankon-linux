# Broken GNOME Appearance After Installing KDE Plasma Desktop

If you tried to install KDE Plasma Desktop, it may break some default configuration for GNOME, even after you uninstall the entire KDE Plasma Desktop packages with `sudo apt autoremove kde-plasma-desktop`.

To restore these, please follow these steps below:

1. You can install `gnome-tweaks` package and restore some configuration to `Adawaita` in Appearance settings there, like Cursor, Icons, and Legacy Applications.
2. Then open GNOME Settings -> Appearance, click Light or Dark appearance back and forth to refresh the UI render in GNOME.
