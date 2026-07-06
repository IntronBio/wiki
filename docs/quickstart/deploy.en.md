---
title: First Deployment Guide
description: AIEdge Micro AI Agent Server — First Deployment Step-by-Step
---

# First Deployment Guide

## Deployment Overview

The complete process for deploying an AIEdge Micro AI Agent Server for the first time:

```mermaid
flowchart LR
    subgraph Hardware Setup
        direction TB
        A[📦 Place Device] --> B[🔌 Connect Hardware] --> C[⚡ Power On] --> D[🖥 Boot Up]
    end
    subgraph Connection & Config
        direction TB
        E[📡 Get Device IP] --> F[💻 SSH Login] --> G[🔑 Configure API Key] --> H[🚀 Start Agents]
    end
    subgraph Go Live
        direction TB
        I[💬 Connect Messaging Channels]
    end
    Hardware Setup --> Connection & Config --> Go Live
```

---

## Step 1: Place the Device

Choose an installation location:

!!! tip "Location Recommendations"
    - :material-check: Well-ventilated, away from heat sources
    - :material-check: Stable, flat surface — avoid vibration
    - :material-check: Near Ethernet port and power outlet
    - :material-check: Keep at least 5cm clearance around the device
    - :material-close: Avoid direct sunlight
    - :material-close: Avoid humid or dusty environments

---

## Step 2: Connect Hardware

1. Plug one end of the Ethernet cable into the device's **RJ45** port and the other into your router's LAN port
2. Connect the power adapter to the device's power port and plug into an outlet
3. The front blue LED will blink and then stay solid — system boot is complete
4. For subsequent operations, choose:
    - **SSH Configuration**: Find the device IP in your router admin panel and SSH from your computer
    - **Direct HDMI**: Connect an HDMI monitor and keyboard to operate directly in the terminal

---

## Step 3: Get the Device IP Address

The device needs an IP address before you can SSH in. Two ways to find it:

=== "Method 1: Router Admin (Recommended)"

    1. Open your router management page (usually `http://192.168.1.1`)
    2. Find the "DHCP Client List" or "Connected Devices"
    3. Look for a device with the hostname `aiedge`
    4. Note its IP address (e.g., `192.168.1.105`)

=== "Method 2: HDMI Display"

    1. Connect the device to a monitor via HDMI
    2. Once booted, you'll see a login prompt on the display
    3. Log in with the default credentials:
    
    ```
    Username: root
    Password: hybio.tech
    ```
    
    !!! tip "Password Input Tip"
        The screen will **not show** any characters while you type the password (no `***` feedback) — this is normal Linux security behavior. Just type it blind and press Enter.
    
    4. After logging in, run:
    
    ```bash
    ip a
    ```
    
    5. Look for the `inet` address under `eth0` — that's your device's LAN IP (e.g., `192.168.1.105`)

---

## Step 4: SSH into the Device

Once you have the IP, open **PowerShell** (Windows) or **Terminal** (macOS / Linux) on your computer and enter:

```bash
# Use the device IP (replace 192.168.1.105 with the actual IP)
ssh root@192.168.1.105
```

!!! info "Default Credentials"
    Username: `root` | Password: `hybio.tech`

    **Note**: The screen won't show any characters while you type the password (Linux security design). Blind-type it and press Enter.

!!! tip "First Login Note"
    On first SSH login, you'll be prompted to change the password. We recommend setting a strong one.

---

## Step 5: Start Network Proxy (Optional)

If you're in China, accessing overseas APIs (Anthropic / OpenAI, etc.) may be restricted. AIEdge comes with **Mihomo (Clash)** transparent proxy pre-installed — configure your subscription to ensure smooth API connectivity.

!!! note "Proxy Subscription"
    You'll need to provide your own Clash subscription. You can purchase a paid subscription or use free subscription links from various providers. The device itself does not include a proxy subscription.

```bash
# 1. Edit Mihomo config, add your subscription link or node info
sudo nano /etc/mihomo/config.yaml

# 2. Start the proxy service
sudo systemctl start mihomo

# 3. Check running status
sudo systemctl status mihomo

# 4. Enable auto-start on boot (recommended)
sudo systemctl enable mihomo

# 5. Verify proxy connectivity
curl -x http://127.0.0.1:7890 https://api.anthropic.com
```

!!! note "Skip Condition"
    If you already have router-level proxy or can access overseas APIs without a proxy, you can skip this step.

---

## Step 6: Configure LLM API Keys

!!! important "Important"
    AIEdge **does not run local models**. All AI capabilities are delivered through cloud LLM APIs. You'll need to configure at least one LLM provider's API Key.

=== "Configure Anthropic Claude"

    ```bash
    # Set Anthropic API Key for agent frameworks
    openclaw config set ANTHROPIC_API_KEY "sk-ant-xxxxxxxxxxxxx"
    hermes config set ANTHROPIC_API_KEY "sk-ant-xxxxxxxxxxxxx"
    
    # Claude Code
    claude config set apiKey "sk-ant-xxxxxxxxxxxxx"
    ```

=== "Configure OpenAI"

    ```bash
    openclaw config set OPENAI_API_KEY "sk-xxxxxxxxxxxxx"
    hermes config set OPENAI_API_KEY "sk-xxxxxxxxxxxxx"
    ```

=== "Configure DeepSeek"

    ```bash
    # DeepSeek is compatible with OpenAI API format
    openclaw config set OPENAI_API_KEY "sk-xxxxxxxxxxxxx"
    openclaw config set OPENAI_BASE_URL "https://api.deepseek.com"
    
    hermes config set provider deepseek
    hermes config set DEEPSEEK_API_KEY "sk-xxxxxxxxxxxxx"
    ```

=== "Configure OpenRouter (Unified Multi-Model Access)"

    ```bash
    openclaw config set OPENROUTER_API_KEY "sk-or-xxxxxxxxxxxxx"
    hermes config set OPENROUTER_API_KEY "sk-or-xxxxxxxxxxxxx"
    ```

!!! tip "Recommendation"
    Use **OpenRouter** as a unified API entry point — one Key to access Anthropic / OpenAI / Google / DeepSeek and all other models, with automatic failover and cost-based routing.

---

## Step 7: Start Agent Services

### Start OpenClaw

```bash
# Install and start OpenClaw daemon
openclaw onboard --install-daemon

# Check status
openclaw status
```

### Start Hermes Agent

```bash
# Initialize and start Hermes
hermes setup
hermes start --daemon

# Check status
hermes status
```

### Using Claude Code

```bash
# Navigate to a project directory and start using it
cd /root/projects
claude "Analyze the code structure of the current project"
```

---

## Step 8: Connect Messaging Channels

Connect your agents to your team's messaging platforms. See the **[Messaging Integration Guide](../features/wechat-integration.md)** for detailed steps. Quick reference:

| Platform | OpenClaw | Hermes Agent | Claude Code |
|------|:--------:|:------------:|:-----------:|
| :material-wechat: WeChat | Official plugin + QR scan | Native iLink + QR scan | Third-party bridge |
| :material-chat: DingTalk | OpenClaw DingTalk plugin | — | — |
| :material-bird: Lark | OpenClaw Lark plugin | — | — |
| :material-slack: Slack | `@openclaw/slack` | — | — |
| :material-send: Telegram | `@openclaw/telegram` | — | — |

[:material-arrow-right-bold: View Full WeChat Integration Guide](../features/wechat-integration.md){ .md-button }

---

## Deployment Verification Checklist

After completing the steps above, verify each item:

- [ ] Device is powered on; blue LED is solid
- [ ] Can SSH into the device (`ssh root@[device IP]`)
- [ ] Default password has been changed
- [ ] Mihomo proxy configured (if needed), network connectivity confirmed (`curl -x http://127.0.0.1:7890 https://api.anthropic.com`)
- [ ] At least one LLM API Key configured
- [ ] OpenClaw daemon running
- [ ] Hermes Agent started successfully
- [ ] Claude Code is callable

!!! success "Congratulations!"
    Once everything is checked, your AIEdge Micro AI Agent Server is ready to use!

---

## Next Step

After first deployment, continue to **[LAN Quick Start](lan-start.md)** to learn about daily usage on your local network.
