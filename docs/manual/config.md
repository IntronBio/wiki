---
title: 基础配置
description: AIEdge 微型AI智能体服务器基础系统配置指南
---

# 基础配置

## 配置方式

所有配置通过 **SSH 命令行** 完成：

```bash
ssh root@[设备IP]
```

默认账户：

| 项目 | 值 |
|------|-----|
| 用户名 | `root` |
| 初始密码 | `hybio.tech`（输入时屏幕不显示，盲输后回车确认） |

---

## 修改密码

首次登录后建议立即修改密码：

```bash
passwd
```

!!! warning "密码要求"
    - 长度：至少 8 位
    - 建议包含：大小写字母 + 数字 + 特殊字符

---

## API Key 管理

### 查看当前配置的 Key

```bash
# OpenClaw
openclaw config list

# Hermes Agent
hermes config show
```

### 添加/更新 API Key

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
    # DeepSeek 兼容 OpenAI API 格式
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

### 切换默认模型

```bash
openclaw config set model "anthropic/claude-sonnet-4-6"
hermes config set model "openrouter/anthropic/claude-sonnet-4-6"
```

---

## 网络配置

### 查看当前 IP

```bash
ip a
# 查看 eth0 的 inet 地址
```

### 设置静态 IP

```bash
# 编辑 netplan 配置
sudo nano /etc/netplan/01-netcfg.yaml

# 示例静态 IP 配置：
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

# 应用配置
sudo netplan apply
```

---

## Mihomo (Clash) 代理管理（可选）

AIEdge 可选安装 Mihomo 作为透明代理，保障国内环境访问海外 LLM API 的链路畅通。

```bash
# 1. 启动代理服务
sudo systemctl start mihomo

# 2. 完全关闭代理服务
sudo systemctl stop mihomo

# 3. 重启代理（修改配置 / 更新订阅后重载）
sudo systemctl restart mihomo

# 4. 查看代理运行状态
sudo systemctl status mihomo

# 5. 设置开机自动启动
sudo systemctl enable mihomo

# 6. 取消开机自启
sudo systemctl disable mihomo

# 7. 实时查看运行日志（排错专用）
sudo journalctl -u mihomo -f
```

!!! tip "配置文件位置"
    修改订阅链接或节点信息：`sudo nano /etc/mihomo/config.yaml`

---

## 智能体服务管理

### 管理 OpenClaw

```bash
# 启动 / 停止 / 重启
sudo systemctl start openclaw
sudo systemctl stop openclaw
sudo systemctl restart openclaw

# 查看状态
sudo systemctl status openclaw

# 查看日志
journalctl -u openclaw -f
```

### 管理 Hermes Agent（可选）

```bash
sudo systemctl start hermes-agent
sudo systemctl stop hermes-agent
sudo systemctl restart hermes-agent
sudo systemctl status hermes-agent
```

### 服务端口说明

| 端口 | 服务 | 用途 |
|------|------|------|
| 18789 | OpenClaw Gateway | OpenClaw API 网关 |
| 11434 | Hermes Agent | Hermes Agent API |
| 22 | SSH | SSH 远程管理 |

---

## 时区与语言

```bash
# 设置时区
sudo timedatectl set-timezone Asia/Shanghai

# 设置系统语言
sudo localectl set-locale LANG=zh_CN.UTF-8
```

---

## 出厂重置

!!! danger "警告"
    出厂重置将**删除所有数据**：API Key 配置、智能体记忆、技能、日志等。操作不可逆！

```bash
# 命令行重置
sudo aiedge-reset --factory
```

硬件强制恢复：长按复位按钮 30 秒以上（蓝色指示灯快闪后松开）。
