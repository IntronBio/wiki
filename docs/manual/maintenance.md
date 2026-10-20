---
title: 日常运维
description: AIEdge 微型AI智能体服务器日常运维指南
---

# 日常运维

## 系统监控

### 命令行监控

```bash
# CPU 和内存占用
htop

# 磁盘使用
df -h

# 所有服务状态
sudo systemctl status mihomo openclaw hermes-agent

# 实时系统日志
journalctl -f
```

---

## 智能体日志管理

### 日志位置

| 日志类型 | 路径 | 内容 |
|:---------|:-----|:-----|
| Mihomo 日志 | `journalctl -u mihomo` | 代理连接、订阅更新 |
| OpenClaw 日志 | `/var/log/openclaw/` | 消息路由、API调用、错误 |
| Hermes Agent 日志 | `/var/log/hermes/` | 代理行为、记忆操作 |
| 系统日志 | `/var/log/syslog` | 操作系统事件 |

### 日志查看

```bash
# 查看 OpenClaw 最新日志
tail -100 /var/log/openclaw/gateway.log

# 实时跟踪
tail -f /var/log/openclaw/gateway.log

# 搜索 API 错误
grep -i "api.*error" /var/log/openclaw/gateway.log
```

---

## 备份与恢复

### 需要备份的内容

| 项目 | 路径 | 重要性 |
|------|------|:------:|
| OpenClaw 数据 | `/root/.openclaw/` | ⭐⭐⭐ |
| Hermes 记忆 | `/root/.hermes/` | ⭐⭐⭐ |
| 自定义技能 | `/root/.openclaw/skills/` | ⭐⭐ |
| 用户项目 | `/root/projects/` | ⭐⭐ |

### 备份操作

```bash
# 创建全量备份
tar -czf /tmp/aiedge-backup-$(date +%Y%m%d).tar.gz \
  /root/.openclaw/ \
  /root/.hermes/

# 复制到 U 盘或 TF 卡
cp /tmp/aiedge-backup-*.tar.gz /mnt/sdcard/
```

---

## OTA 系统升级

```bash
# 升级 OpenClaw
npm update -g openclaw

# 升级 Hermes Agent
pip install --upgrade hermes-agent

# 升级 Claude Code
npm update -g @anthropic-ai/claude-code

# 升级系统软件包（Armbian / Debian）
sudo apt update && sudo apt upgrade -y
```

!!! tip "升级前准备"
    - 升级前做好数据备份
    - 大版本升级后需验证 API Key 配置和消息通道连通性

---

## 性能优化建议

| 优化项 | 操作 | 效果 |
|--------|------|------|
| 日志清理 | 定期清理旧日志 | 释放磁盘空间 |
| 服务精简 | 不用的消息通道插件及时卸载 | 降低内存占用 |
| 定期重启 | 设置每周日凌晨定时重启 | 清理内存碎片 |
| 磁盘监控 | `df -h` 定期检查 | 避免写满存储 |

### 设置定时重启

```bash
# 编辑 crontab
crontab -e

# 添加定时重启（每周日凌晨 3:00）
0 3 * * 0 /sbin/shutdown -r now
```

---

## 常见运维操作速查

| 操作 | 命令 |
|------|------|
| 查看系统版本 | `cat /etc/armbian-release && uname -r` |
| 查看 OpenClaw 版本 | `openclaw --version` |
| 查看 Hermes 版本 | `hermes --version` |
| 查看 Claude Code 版本 | `claude --version` |
| 重启 OpenClaw | `sudo systemctl restart openclaw` |
| 重启 Hermes | `sudo systemctl restart hermes-agent` |
| 查看磁盘空间 | `df -h` |
| 查看 IP | `ip a` |
| 网络连通测试 | `ping -c 4 openrouter.ai` |
