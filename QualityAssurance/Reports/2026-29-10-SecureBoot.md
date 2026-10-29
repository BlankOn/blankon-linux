### BlankOn Linux Project : 2026-10-27-jahitan-6

Report created at: 29/10/2026, 15.10

| Passed | Failed | On test | Unassigned |
| :----: | :----: | :-----: | :--------: |
|   4    |   0    |    0    |     0      |

| Secure Boot |           Tester           | Result | Notes |
| ----------- | :------------------------: | :----: | ----: |
| Secureboot  | mandexx@wearehackerone.com |  PASS  |       |

| NO  | Step                                                    |               Expectation               | Result | Notes |
| --- | ------------------------------------------------------- | :-------------------------------------: | :----: | ----: |
| 1   | EFI + Secure Boot ENABLED Di VirtBox                    | Boot sukses ke GRUB -> masuk ke BlankOn |  PASS  |       |
| 2   | Cek status Secure Boot dari firmware mokutil --sb-state |           SecureBoot Enabled            |  PASS  |       |
| 3   | Cek Lockdown cat /sys/kernel/security/lockdown          |     none [integriy] Confidentiality     |  PASS  |       |
| 4   | dmesg grep -iE 'secure boot'                            |           Kernel is lockdown            |  PASS  |       |
