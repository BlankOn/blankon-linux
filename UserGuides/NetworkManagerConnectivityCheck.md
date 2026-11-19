# NetworkManager Connectivity Check and the Question Mark LAN Icon

## Symptom

The wired (LAN) icon in the panel shows a **question mark** even though the network cable is connected and internet access works normally.

## Cause

NetworkManager periodically probes a built-in web endpoint to decide whether the machine has real internet access. If that probe fails or times out, the connection is reported as `limited` (also shown as `connected (site only)`), even when DHCP, the default gateway, DNS and internet access are all working. This is common in regions where the default check host is slow or blocked.

## How to confirm it is only a false warning

Check the connection state:

```bash
nmcli general status
```

A false warning looks like this:

```text
STATE                  CONNECTIVITY
connected (site only)  limited
```

Then verify that the network itself is healthy:

```bash
ip -br addr
ip route
ping -c 2 8.8.8.8
ping -c 2 google.com
```

If the interface is `UP` with an IP address, a default route exists, and both pings return `0% packet loss`, the connection is fine and only the connectivity check is misreporting it.

If `CONNECTIVITY` is `none` and the pings fail, the problem is a real one: check the cable, router or ISP instead.

## Workaround

Disable the connectivity probe by creating the following configuration file:

```bash
sudo mkdir -p /etc/NetworkManager/conf.d
sudo tee /etc/NetworkManager/conf.d/20-connectivity.conf <<'EOF'
[connectivity]
uri=
EOF
sudo systemctl restart NetworkManager
```

After a few seconds, check again:

```bash
nmcli general status
```

The result should now show `CONNECTIVITY: full`, and the panel icon should return to a normal connected LAN icon.

This change only stops NetworkManager from second-guessing a working connection. It does not change how the machine connects to the network and it survives a reboot.
