# Date, Time, and Timezone

## Known Issues

In rare occasion, when we try to change the computer's date and time, the values revert to their previous settings, even after disabling `Automatic Time Zone` in GNOME Settings.

This issue may be related to automatic time synchronization, the system time service, or GNOME's permissions to change the system clock.

### Workaround

We can manually set the system date and time through the terminal using the following command:

```bash
sudo date -s "2026-10-28 13:30:00"
```
