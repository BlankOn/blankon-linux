# Troubleshooting Undetected Wi-Fi Devices

If your Wi-Fi device is not detected or does not appear in the network settings, follow this guide to identify the hardware and find the appropriate driver.

## 1. Identify Your Wi-Fi Device

Open a terminal and run:

```bash
lspci -nnk
```

Look for a line containing `Network controller` or `Wireless`.

For example:

```text
01:00.0 Network controller [0280]: Broadcom Inc. and subsidiaries BCM43142 802.11b/g/n [14e4:4365] (rev 01)
    Subsystem: AzureWave Device [1a3b:2155]
```

This tells us that the device is a **Broadcom BCM43142** with hardware ID `14e4:4365`.

The hardware ID helps identify the correct driver.

## 2. Find the Appropriate Driver

Visit the [BlankOn Packages](https://packages.blankonlinux.id/) website and search for the driver matching your Wi-Fi hardware.

Different Wi-Fi chipsets require different drivers. Follow the appropriate section below.

### 2.1. Broadcom BCM43142

For Broadcom BCM43142, search for these packages in [BlankOn Packages](https://packages.blankonlinux.id/) and install the following in order:

1. `libiw` - [Download](https://arsip.blankonlinux.id/sinambung/pool/main/w/wireless-tools/libiw30t64_30~pre9-23_amd64.deb)
2. `wireless-tools` - [Download](https://arsip.blankonlinux.id/sinambung/pool/main/w/wireless-tools/wireless-tools_30~pre9-23_amd64.deb)
3. `broadcom-sta-dkms` - [Download](https://arsip.blankonlinux.id/sinambung/pool/restricted/b/broadcom-sta/broadcom-sta-dkms_6.30.223.271-32_amd64.deb)
4. `broadcom-sta-common` - [Download](https://arsip.blankonlinux.id/sinambung/pool/restricted/b/broadcom-sta/broadcom-sta-common_6.30.223.271-32_amd64.deb)

Download and install each `.deb` package one by one.

You can install downloaded packages using:

```bash
sudo apt install ./package-name.deb
```

Make sure you have the required kernel headers and DKMS build dependencies installed.

## 3. Restart Your Computer

After installing the driver, restart your computer:

```bash
sudo reboot
```

Check whether your Wi-Fi device is now detected.

## 4. Still Not Working?

If your Wi-Fi device is still not detected:

- Check the hardware ID using `lspci -nnk`.
- Make sure the correct driver is installed.
- Check whether the driver supports your current kernel version.
- Ask for help in the BlankOn community and include your `lspci -nnk` output.

**Important:** This guide is a collection of troubleshooting solutions for different Wi-Fi chipsets. The Broadcom example above is only one possible solution. Always identify your hardware before installing a driver.
