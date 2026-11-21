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

Point NetworkManager's connectivity check at a reliable, plain-HTTP endpoint:

```bash
sudo mkdir -p /etc/NetworkManager/conf.d
sudo tee /etc/NetworkManager/conf.d/20-connectivity.conf <<'EOF'
[connectivity]
uri=http://nmcheck.gnome.org/check_network_status.txt
interval=15
timeout=5
EOF
sudo systemctl restart NetworkManager
```

The endpoint returns the text `NetworkManager is online`, which is what NetworkManager expects. Using plain HTTP instead of HTTPS avoids certificate failures when the system clock has not synced yet during boot.

Check the result:

```bash
nmcli general status
```

It should show `CONNECTIVITY: full`, with the panel icon back to a normal connected LAN icon. Because the check stays enabled, NetworkManager re-evaluates on its own after each reboot — usually within 15 seconds of the network coming up.

**Do not use `uri=` (an empty value) as a workaround.** That disables the check completely, so if NetworkManager starts in the `limited` state it never re-evaluates and the question mark returns after every reboot. Restarting NetworkManager manually only hides it temporarily.
