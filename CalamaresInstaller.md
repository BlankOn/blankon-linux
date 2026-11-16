# Calamares

Calamares is Linux installer build for Debian: https://salsa.debian.org/live-team/calamares-settings-debian

Even though the `calamares` program resides on the `calamares` package, it is the `calamares-settings` package that hold the whole program. Any customization against the installer should be addressed to this package.

We manage the `calamares-settings` package here as `calamares-settings-blankon`: https://github.com/blankon-packages/calamares-settings-blankon/

## Run

Calamares need to be run with sudo. For full debug mode, run with this command:

```
sudo calamares -D8 | sudo tee calamares.log
```

## Log on production

If users get failed installation, please ask them to handover this log file:

```
/tmp/calamares-blankon.log
```
