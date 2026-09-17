# Migrate from Trixie to BlankOn Sinambung

## 1. Install the BlankOn keyring

```
wget https://arsip.blankonlinux.id/sinambung/pool/main/b/blankon-keyring/blankon-keyring_2020.11.25-4.5_all.deb
sudo dpkg -i blankon-keyring_2020.11.25-4.5_all.deb
```


## Replace the APT source

```
deb [signed-by=/usr/share/keyrings/blankon-archive-keyring.gpg] https://arsip.blankonlinux.id/sinambung sinambung main extras restricted restricted-firmware
```

## Upgrade the distribution to Sinambung

```
sudo apt update
sudo apt dist-upgrade
```
