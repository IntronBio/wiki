---
title: Network Setup
description: AIEdge LAN Network Configuration Guide
---

# Network Setup

## Network Architecture

The AIEdge device supports the following network access methods:

| Method | Throughput | Stability | Recommended For |
|------|--------|--------|----------|
| Wired 100Mbps (Recommended) | 100 Mbps | ⭐⭐⭐⭐⭐ | Fixed installation, production |
| Wi-Fi 5 | Up to 433 Mbps | ⭐⭐⭐⭐ | Temporary deployment, mobile |

---

## Wired Network Configuration

### Automatic IP (DHCP) — Default Mode

The device automatically obtains an IP address from the router on startup — **no configuration needed in most cases**.

!!! tip "Confirm the IP Address"
    In your router admin panel's DHCP client list, look for a device with hostname `aiedge` to find its IP address.

### Configure Static IP

When you need a fixed IP (e.g., for port forwarding or firewall whitelisting):

```bash
# Edit netplan configuration
sudo nano /etc/netplan/01-netcfg.yaml

# Example:
# network:
#   ethernets:
#     eth0:
#       dhcp4: no
#       addresses:
#         - 192.168.1.100/24
#       routes:
#         - to: default
#           via: 192.168.1.1
#       nameservers:
#         addresses: [192.168.1.1, 223.5.5.5]
#   version: 2

# Apply configuration
sudo netplan apply
```

---

## Wi-Fi Configuration

### First-Time Wi-Fi Setup

If the device isn't connected via Ethernet, it will create a setup hotspot on first boot:

1. Search for Wi-Fi hotspot on your phone/computer: `AIEdge-Setup-XXXX`
2. Connect to the hotspot (no password required)
3. A configuration page should open automatically in your browser (or visit `http://192.168.4.1`)
4. Select your Wi-Fi network and enter the password
5. The device connects and the hotspot shuts down

### Changing Wi-Fi Later

SSH into the device and use `nmtui` or edit the netplan config.

---

## Advanced Network Settings

### Port Reference

| Port | Protocol | Service | Configurable |
|------|------|------|:--:|
| 18789 | HTTP | OpenClaw API Gateway | ✅ |
| 11434 | HTTP | Hermes Agent API | ✅ |
| 22 | SSH | SSH Remote Management | ✅ |

### Firewall Configuration

Default firewall rules:

```
Inbound:  Allow 22 (SSH), 18789 (OpenClaw), 11434 (Hermes) — LAN only
          Deny all external access
Outbound: Allow all (HTTPS 443 for LLM API calls)
```

---

## Network Diagnostics

### Command-Line Diagnostics

```bash
# Check IP address
ip a

# Check routing table
ip route show

# Test connectivity
ping -c 4 192.168.1.1
ping -c 4 openrouter.ai

# DNS resolution test
nslookup api.deepseek.com
```

---

## Network Troubleshooting

| Symptom | Troubleshooting Steps |
|------|----------|
| Can't obtain IP | ① Check Ethernet cable is plugged in ② Restart router DHCP service ③ Try manual static IP |
| Can't call LLM APIs | ① `ping openrouter.ai` ② Check corporate firewall ③ Verify API Key is valid |
| Not discoverable on LAN | ① Confirm same subnet ② Disable client firewall ③ Try direct IP connection |
| Intermittent disconnection | ① Replace Ethernet cable ② Try different router port ③ Check switch status |
