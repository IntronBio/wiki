---
title: Routine Maintenance
description: AIEdge Micro AI Agent Server — Daily Maintenance Guide
---

# Routine Maintenance

## System Monitoring

### Command-Line Monitoring

```bash
# CPU and memory usage
htop

# Disk usage
df -h

# All service statuses
sudo systemctl status mihomo openclaw hermes-agent

# Real-time system logs
journalctl -f
```

---

## Agent Log Management

### Log Locations

| Log Type | Path | Content |
|:---------|:-----|:-----|
| Mihomo logs | `journalctl -u mihomo` | Proxy connections, subscription updates |
| OpenClaw logs | `/var/log/openclaw/` | Message routing, API calls, errors |
| Hermes Agent logs | `/var/log/hermes/` | Agent behavior, memory operations |
| System logs | `/var/log/syslog` | OS events |

### Viewing Logs

```bash
# View recent OpenClaw logs
tail -100 /var/log/openclaw/gateway.log

# Real-time tail
tail -f /var/log/openclaw/gateway.log

# Search for API errors
grep -i "api.*error" /var/log/openclaw/gateway.log
```

---

## Backup & Recovery

### What to Back Up

| Item | Path | Importance |
|------|------|:------:|
| OpenClaw data | `/root/.openclaw/` | ⭐⭐⭐ |
| Hermes memory | `/root/.hermes/` | ⭐⭐⭐ |
| Custom skills | `/root/.openclaw/skills/` | ⭐⭐ |
| User projects | `/root/projects/` | ⭐⭐ |

### Backup Operations

```bash
# Create full backup
tar -czf /tmp/aiedge-backup-$(date +%Y%m%d).tar.gz \
  /root/.openclaw/ \
  /root/.hermes/

# Copy to USB drive or TF card
cp /tmp/aiedge-backup-*.tar.gz /mnt/sdcard/
```

---

## OTA System Upgrades

```bash
# Upgrade OpenClaw
npm update -g openclaw

# Upgrade Hermes Agent
pip install --upgrade hermes-agent

# Upgrade Claude Code
npm update -g @anthropic-ai/claude-code

# Upgrade system packages (Armbian / Debian)
sudo apt update && sudo apt upgrade -y
```

!!! tip "Before Upgrading"
    - Back up your data before upgrading
    - After major version upgrades, verify API Key configuration and messaging channel connectivity

---

## Performance Optimization Tips

| Optimization | Action | Effect |
|--------|------|------|
| Log cleanup | Regularly clean old logs | Free disk space |
| Service trimming | Uninstall unused messaging channel plugins | Reduce memory usage |
| Regular restarts | Schedule weekly automatic restart (e.g., Sunday early AM) | Clear memory fragmentation |
| Disk monitoring | Regularly check `df -h` | Prevent storage from filling up |

### Schedule Automatic Restarts

```bash
# Edit crontab
crontab -e

# Add scheduled restart (every Sunday at 3:00 AM)
0 3 * * 0 /sbin/shutdown -r now
```

---

## Common O&M Commands Quick Reference

| Operation | Command |
|------|------|
| Check system version | `cat /etc/armbian-release && uname -r` |
| Check OpenClaw version | `openclaw --version` |
| Check Hermes version | `hermes --version` |
| Check Claude Code version | `claude --version` |
| Restart OpenClaw | `sudo systemctl restart openclaw` |
| Restart Hermes | `sudo systemctl restart hermes-agent` |
| Check disk space | `df -h` |
| Check IP | `ip a` |
| Network connectivity test | `ping -c 4 openrouter.ai` |
