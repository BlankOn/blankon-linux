# Debug

The best way to identify the root cause of an AppImage launch issue is to launch the AppImage from a terminal, for example:

```
./SomeApp-AppImage
```

The terminal output will usually reveal why the AppImage fails to launch or crashes.

# Known Issues

## Master PDF Editor

https://github.com/ryuuzaki42/MasterPDFEditor_AppImage_4

The app cannot launch immediately because it expects the system to have libQt5PrintSupport.so.5. You can install it via APT:

```
sudo apt install libqt5printsupport5
```
