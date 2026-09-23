# Managing Repository

### Arsip - Sinambung

This is a production repository, where majority of the packages supposed to be stable.

Path: `/etc/apt/sources.list.d/blankon.sources`

```
Types: deb deb-src
URIs: http://arsip.blankonlinux.id/sinambung/
Suites: sinambung
Components: main extras restricted restricted-firmware
Signed-By: /usr/share/keyrings/blankon-archive-keyring.gpg
```

### Arsipd-dev - Sinambung

This is the development repository, which is synced with Sid from time to time and may break your system. This repository is used by the development team to test packages and system changes.

Path: `/etc/apt/sources.list.d/blankon.sources`

```
Types: deb deb-src
URIs: http://arsip-dev.blankonlinux.id/sinambung/
Suites: sinambung
Components: main extras restricted restricted-firmware
Signed-By: /usr/share/keyrings/blankon-archive-keyring.gpg
```

#### Arsip - Verbeek and Arsipd-dev - Verbeek

Verbeek has been discontinued and continue as ISO code name. It is no longer has any dedicated repository for its own.

If you are still in Verbeek (which you install your system between Q1-Q2 2026), you can move to Sinambung by editing your `/etc/apt/sources.list.d/blankon.sources` to Arsip - Sinambung (please see above)

