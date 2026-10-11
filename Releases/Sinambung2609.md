# BlankOn Sinambung 26.09

This page is for people who want to download and install BlankOn. BlankOn Sinambung 26.09 is the current installation image (ISO file) of BlankOn Linux, published on 21 September 2026. The page explains the release name, where to download the image, how to check that the download is complete and correct, and how to start the installation.

## What the Name Means

The file name is `blankon-sinambung-26.09-verbeek-amd64.iso`. Each part has a meaning:

- **Sinambung** is the BlankOn distribution. It is a rolling release based on Debian Sid: it gets new packages in tested batches, so there are no large version upgrades.
- **26.09** is the date of this image: 2026, September.
- **Verbeek** is the code name of the BlankOn ISO image.
- **amd64** means the image is for 64-bit PCs with an Intel or AMD processor.

After you install BlankOn, your system gets updates from Arsip, the BlankOn package repository. You do not need to download a new image to stay up to date. See [Managing Repository](https://github.com/BlankOn/blankon-linux/blob/main/UserGuides/ManagingRepository.md).

## Download

Download the ISO file from Jahitan, the BlankOn image server:

https://jahitan.blankonlinux.id/releases/current/

This directory always has the newest image. When a newer image is published, its file names have a newer date than `26.09`.

| File | What it is |
|---|---|
| `blankon-sinambung-26.09-verbeek-amd64.iso` | The installation image, about 3.3 GB |
| `blankon-sinambung-26.09-verbeek-amd64.iso.sha256sum` | The SHA-256 checksum of the image |

The same directory also has `.packages`, `.files` and `.contents` files. They list what is inside the image. You do not need them to install BlankOn.

## Check the Download

A checksum shows that the file downloaded completely and has not changed. Download the checksum file into the same directory as the ISO file, then run the command for your system.

On Linux:

```
sha256sum -c blankon-sinambung-26.09-verbeek-amd64.iso.sha256sum
```

The result must be `blankon-sinambung-26.09-verbeek-amd64.iso: OK`. If it says `FAILED`, download the ISO file again.

On Windows, in PowerShell:

```
Get-FileHash blankon-sinambung-26.09-verbeek-amd64.iso -Algorithm SHA256
```

Compare the hash in the result with the long code in the checksum file. They must be the same. Upper case and lower case letters do not matter.

## Install

1. Write the ISO file to a USB drive of 4 GB or more. On Linux you can use GNOME Disks ("Restore Disk Image"). On Windows you can use a tool such as Rufus or balenaEtcher. If Rufus asks which mode to use, choose "DD Image" mode. All data on the USB drive is deleted.
2. Turn off Secure Boot in your computer's firmware settings. You can turn it on again after the installation. See [Secure Boot](https://github.com/BlankOn/blankon-linux/blob/main/UserGuides/SecureBoot.md).
3. Start the computer from the USB drive. BlankOn starts as a live system, which you can try without installing.
4. Start the installer from the live system and follow the steps.

If you find a problem, report it at https://github.com/BlankOn/blankon-linux/issues.
