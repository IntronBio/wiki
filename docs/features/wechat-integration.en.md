---
title: Messaging Integration Guide
description: AIEdge Agent Integration with WeChat/DingTalk/Lark/Slack — Detailed Guide
---

# Messaging Integration Guide

The three agent frameworks pre-installed on AIEdge each have different messaging channel integration methods. Using **WeChat** as an example, this guide covers the integration steps for each framework.

---

## 1. OpenClaw → WeChat

OpenClaw (latest ≥ 2026.6.x) integrates with WeChat via the official plugin — just scan a QR code to bind.

### 1. Enable the WeChat Plugin

```bash
# Install the official WeChat plugin
openclaw plugins install @tencent-weixin/openclaw-weixin --force

# Enable the plugin
openclaw config set plugins.entries.openclaw-weixin.enabled true

# Restart the gateway
openclaw gateway restart
```

### 2. Scan to Bind

```bash
# Display the WeChat login QR code
openclaw channels login --channel openclaw-weixin
```

The terminal will output a QR code link — open it in a browser, scan with WeChat on your phone, and approve the ClawBot authorization to complete binding.

### 3. Multi-Account Setup (Optional)

```bash
# Isolate sessions per WeChat account
openclaw config set session.dmScope per-account-channel-peer
```

### 4. Troubleshooting

| Issue | Solution |
|------|----------|
| QR scan unresponsive | Run `openclaw gateway restart` |
| Old plugin compatibility bugs | Ensure version ≥ 2026.3 (pre-installed on AIEdge) |

---

## 2. Hermes Agent → WeChat

Hermes Agent (v0.6.0+) natively supports Tencent iLink — no public IP needed, stable long-polling.

### 1. Install Dependencies

```bash
# Install WeChat adapter dependencies
pip install aiohttp cryptography qrcode
```

### 2. Configure WeChat Channel

```bash
# Interactive gateway setup wizard
hermes setup gateway
```

Steps:
1. Select **Weixin** from the list (Space to check) → Enter to confirm
2. The terminal outputs a QR code link — open in browser, scan with WeChat to authorize
3. Permission config: enable DMs; group chat is unstable with the official channel, suggest leaving it off
4. Save config and restart the gateway

```bash
hermes gateway restart
```

### 3. Key Limitations

!!! warning "Note"
    - Only **direct messages** are reliably supported; group messages may be lost
    - The same WeChat account **cannot** be simultaneously bound to both OpenClaw and Hermes — scanning will auto-unbind the old binding

---

## 3. Claude Code → WeChat

Claude Code has no native WeChat channel. Use the open-source bridge tool `claude-code-wechat-channel` for message forwarding.

```
WeChat ClawBot → Local Bridge Service → Claude Code CLI
```

### Integration Steps

Ensure the local `claude` command works (Anthropic API configured).

```bash
# Initialize WeChat binding
npx claude-code-wechat-channel setup
```

Scan to authorize WeChat ClawBot — credentials are saved to `~/.claude/channels/wechat/account.json`.

```bash
# Install MCP service and run in background
npx claude-code-wechat-channel install

# Start background listener
bash ~/.claude/skills/claude-to-im/scripts/daemon.sh start
```

---

## WeChat Integration Comparison

| Dimension | OpenClaw | Hermes Agent | Claude Code |
|------|:--------:|:------------:|:-----------:|
| Integration Method | Official plugin + QR scan | Native iLink + QR scan | Third-party bridge |
| Public IP Required | No | No | No |
| Direct Messages | ✅ | ✅ | ✅ |
| Group Chat | ✅ | ⚠️ Unstable | ⚠️ Limited |
| Multi-Account | ✅ | ❌ | ❌ |
| Recommended For | Full-featured team use | Personal DM chat | Code-related Q&A |

---

## More Channels

After completing WeChat integration, you can integrate other platforms (DingTalk, Lark, Slack, Telegram, etc.) following the same pattern. Refer to each framework's official documentation:

- OpenClaw: `openclaw plugins list` to see available plugins
- Hermes Agent: `hermes setup gateway` to see supported channels
- Claude Code: Community bridge tool ecosystem
