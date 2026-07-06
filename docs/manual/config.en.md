---
title: Basic Configuration
description: AIEdge Micro AI Agent Server — Basic System Configuration Guide
---

# Basic Configuration

## Configuration Method

All configuration is done via **SSH command line**:

```bash
ssh root@[device IP]
```

Default credentials:

| Item | Value |
|------|-----|
| Username | `root` |
| Initial Password | `hybio.tech` (no echo while typing; blind-type and press Enter) |

---

## Change Password

Change the password immediately after first login:

```bash
passwd
```

!!! warning "Password Requirements"
    - Length: at least 8 characters
    - Recommended: mix of upper/lowercase letters + digits + special characters

---

## API Key Management

### View Currently Configured Keys

```bash
# OpenClaw
openclaw config list

# Hermes Agent
hermes config show
```

### Add/Update API Keys

=== "Anthropic Claude"

    ```bash
    openclaw config set ANTHROPIC_API_KEY "sk-ant-xxxxxxxxxxxxx"
    hermes config set ANTHROPIC_API_KEY "sk-ant-xxxxxxxxxxxxx"
    ```

=== "OpenAI"

    ```bash
    openclaw config set OPENAI_API_KEY "sk-xxxxxxxxxxxxx"
    hermes config set OPENAI_API_KEY "sk-xxxxxxxxxxxxx"
    ```

=== "DeepSeek"

    ```bash
    # DeepSeek is compatible with OpenAI API format
    openclaw config set OPENAI_API_KEY "sk-xxxxxxxxxxxxx"
    openclaw config set OPENAI_BASE_URL "https://api.deepseek.com"

    hermes config set provider deepseek
    hermes config set DEEPSEEK_API_KEY "sk-xxxxxxxxxxxxx"
    ```

=== "OpenRouter"

    ```bash
    openclaw config set OPENROUTER_API_KEY "sk-or-xxxxxxxxxxxxx"
    hermes config set OPENROUTER_API_KEY "sk-or-xxxxxxxxxxxxx"
    ```

### Switch Default Model

```bash
openclaw config set model "anthropic/claude-sonnet-4-6"
hermes config set model "openrouter/anthropic/claude-sonnet-4-6"
```

---

## Network Configuration

### Check Current IP

```bash
ip a
# Look for the inet address under eth0
```

### Set Static IP

```bash
# Edit netplan configuration
sudo nano /etc/netplan/01-netcfg.yaml

# Example static IP config:
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

# Apply the configuration
sudo netplan apply
```

---

## Mihomo (Clash) Proxy Management

AIEdge comes with Mihomo pre-installed as a transparent proxy, ensuring reliable access to overseas LLM APIs from within China.

```bash
# 1. Start proxy service
sudo systemctl start mihomo

# 2. Stop proxy service
sudo systemctl stop mihomo

# 3. Restart proxy (after config changes / subscription updates)
sudo systemctl restart mihomo

# 4. Check proxy status
sudo systemctl status mihomo

# 5. Enable auto-start on boot
sudo systemctl enable mihomo

# 6. Disable auto-start on boot
sudo systemctl disable mihomo

# 7. Real-time log monitoring (for debugging)
sudo journalctl -u mihomo -f
```

!!! tip "Config File Location"
    To modify subscription links or node info: `sudo nano /etc/mihomo/config.yaml`

---

## Agent Service Management

### Managing OpenClaw

```bash
# Start / Stop / Restart
sudo systemctl start openclaw
sudo systemctl stop openclaw
sudo systemctl restart openclaw

# Check status
sudo systemctl status openclaw

# View logs
journalctl -u openclaw -f
```

### Managing Hermes Agent

```bash
sudo systemctl start hermes-agent
sudo systemctl stop hermes-agent
sudo systemctl restart hermes-agent
sudo systemctl status hermes-agent
```

### Service Port Reference

| Port | Service | Purpose |
|------|------|------|
| 18789 | OpenClaw Gateway | OpenClaw API gateway |
| 11434 | Hermes Agent | Hermes Agent API |
| 22 | SSH | SSH remote management |

---

## Timezone & Locale

```bash
# Set timezone
sudo timedatectl set-timezone Asia/Shanghai

# Set system locale
sudo localectl set-locale LANG=en_US.UTF-8
```

---

## Factory Reset

!!! danger "Warning"
    Factory reset will **delete all data**: API Key configurations, agent memory, skills, logs, etc. This operation is irreversible!

```bash
# Command-line reset
sudo aiedge-reset --factory
```

Hardware forced reset: press and hold the reset button for 30+ seconds (release when the blue LED begins fast-blinking).
