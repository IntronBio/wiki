---
title: Frequently Asked Questions
description: AIEdge Micro AI Agent Server — FAQ & Troubleshooting
---

# ❓ Frequently Asked Questions

## Device Startup

### Q1: The blue indicator light doesn't turn on after powering up?

**Troubleshooting steps**:

1. Check that the Type-C power cable is firmly connected
2. Try a different power outlet
3. If the above checks pass but the light still won't turn on, contact after-sales for replacement

---

### Q2: The blue light keeps flashing for more than 2 minutes after power-on instead of staying solid?

**Possible cause**: System boot failure.

**Solution**:

1. Unplug the power and wait 30 seconds
2. Re-plug and restart
3. If it still won't boot, press and hold the reset button for 10 seconds to force a reset
4. Contact technical support for system recovery firmware

---

### Q3: How do I factory reset?

Command: `sudo aiedge-reset --factory`

!!! warning "Note"
    Factory reset will erase all API Key configurations, agent memory, and data.

---

## Network Connectivity

### Q4: Can't access the device via `aiedge.local`?

**Cause**: Windows does not support mDNS by default.

**Solution**: Use the IP address to SSH directly; check your router admin panel for the device IP (hostname: `aiedge`), or connect an HDMI display, log in, and run `ip a` to see the eth0 address.

---

### Q5: The device boots but can't obtain an IP address?

**Troubleshooting steps**:

1. Check that the Ethernet cable is properly connected
2. Check that the router's DHCP service is enabled
3. Try a different Ethernet cable
4. Try manually setting a static IP

---

### Q6: Agents can't call cloud APIs?

**Troubleshooting steps**:

1. Confirm the device can reach the internet: `ping openrouter.ai`
2. Check whether the API Key is configured: `aiedge config list-api-keys`
3. Check whether the API Key is valid (balance/limit)
4. Check whether your corporate firewall blocks HTTPS outbound
5. Check agent logs: `tail -f /var/log/openclaw/gateway.log`

---

## Agent Usage

### Q7: OpenClaw fails to start?

**Common causes**:

| Cause | Solution |
|------|----------|
| Node.js version too old | `node --version` — confirm ≥ 22 |
| Port already in use | `sudo lsof -i :18789` to check |
| API Key not configured | `openclaw config list` to check |

```bash
# View detailed error logs
journalctl -u openclaw -n 50
```

---

### Q8: Hermes Agent unresponsive?

**Troubleshooting steps**:

1. Check service status: `sudo systemctl status hermes-agent`
2. Check API Key configuration: `hermes config show`
3. Restart the service: `sudo systemctl restart hermes-agent`
4. Check logs: `journalctl -u hermes-agent -n 50`

---

### Q9: Not receiving agent replies in messaging channels?

**Troubleshooting steps**:

1. Confirm the messaging channel plugin is installed: `openclaw plugins list`
2. Confirm channel configuration is correct (Webhook URL, Token, etc.)
3. Check whether the channel is online: `openclaw channels status`
4. Test API connectivity by sending a test message

---

### Q10: What if I run out of memory?

**Solutions**:

1. Disable unused messaging channel plugins
2. Reduce the number of concurrently active agents
3. Regularly clean up log files
4. Schedule periodic restarts
5. Upgrade to the 4GB model

---

## System Maintenance

### Q11: The device gets slow after running for a long time?

**Solutions**:

1. Schedule a weekly automatic restart (recommended early morning)
2. Clean up old logs to free disk space
3. Check if too many messaging channels are active

---

### Q12: How do I upgrade agent software?

```bash
# Upgrade OpenClaw
npm update -g openclaw

# Upgrade Hermes
pip install --upgrade hermes-agent

# Upgrade Claude Code
npm update -g @anthropic-ai/claude-code
```

---

### Q13: What if I forget the root password?

1. Press and hold the reset button for 30 seconds to restore factory settings
2. Log in with the default credentials: `root` / `hybio.tech` (no echo while typing; press Enter after blind-typing)
3. Reconfigure your API Key

!!! warning "Note"
    Factory reset will erase all data.

---

## Purchasing & After-Sales

### Q14: How to choose between 1GB / 2GB / 4GB?

| Scenario | Recommended |
|------|------|
| Personal dev/learning | 1GB |
| Small team CS/automation | 2GB |
| Department-level multi-agent | 4GB |
| Large-scale multi-scenario | Multiple 4GB units + zoning |

---

### Q15: How to contact after-sales support?

- :material-email: Email: info@hybio.tech
- :material-phone: Tech Support: 19262160086 (also on WeChat)
- :material-phone: Phone: 17501621170
- :material-clock: Business hours: Mon–Fri 9:00–18:00 (Beijing Time)
- Emergency faults: response within 4 hours

---

## Still Unresolved?

Contact technical support with the following information ready:

- Device serial number (on the bottom label)
- Ubuntu version: `lsb_release -a`
- Agent versions: `openclaw --version && hermes --version && claude --version`
- Description of the issue (including screenshots/logs)
- Troubleshooting steps already attempted
