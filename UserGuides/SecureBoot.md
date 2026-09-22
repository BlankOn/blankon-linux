# Secure Boot

BlankOn Linux Sinambung support Secure Boot but only after the OS is installed to the system. Before using the Live USB, you need to disable the Secure Boot temporarily. After installation succeed, you can enable it again.

You can confirm Secure Boot by using this command:

```
$ sudo mokutil --sb-state

[sudo] password for user: 
SecureBoot enabled
```
