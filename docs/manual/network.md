---
title: 局域网联网设置
description: AIEdge 局域网网络配置详解
---

# 局域网联网设置

## 网络架构

AIEdge 设备支持以下网络接入方式：

| 方式 | 吞吐量 | 稳定性 | 推荐场景 |
|------|--------|--------|----------|
| 有线百兆（推荐） | 100 Mbps | ⭐⭐⭐⭐⭐ | 固定安装、生产环境 |
| Wi-Fi 5 | 最高 433 Mbps | ⭐⭐⭐⭐ | 临时部署、移动场景 |

---

## 有线网络配置

### 自动获取 IP（DHCP）— 默认模式

设备启动后自动从路由器获取 IP 地址，**绝大多数场景无需修改**。

!!! tip "确认 IP 地址"
    在路由器管理后台的 DHCP 客户端列表中查找主机名为 `aiedge` 的设备，即可获得 IP 地址。

### 配置静态 IP

当需要固定设备 IP 地址时（如配置端口转发、防火墙白名单等场景）：

```bash
# 编辑 netplan 配置
sudo nano /etc/netplan/01-netcfg.yaml

# 示例：
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

## Wi-Fi 配置

### 首次配置 Wi-Fi

如果设备未连接有线网络，首次启动会创建配置热点：

1. 用手机/电脑搜索 Wi-Fi 热点：`AIEdge-Setup-XXXX`
2. 连接热点（无密码）
3. 浏览器自动弹出配置页面（或访问 `http://192.168.4.1`）
4. 选择您环境的 Wi-Fi 并输入密码
5. 设备自动连接，热点关闭

### 后续修改 Wi-Fi

SSH 登录后使用 `nmtui` 或编辑 netplan 配置。

---

## 高级网络设置

### 端口说明

| 端口 | 协议 | 服务 | 可修改 |
|------|------|------|:--:|
| 18789 | HTTP | OpenClaw API 网关 | ✅ |
| 11434 | HTTP | Hermes Agent API | ✅ |
| 22 | SSH | SSH 远程管理 | ✅ |

### 防火墙配置

默认防火墙规则：

```
入站：允许 22（SSH）、18789（OpenClaw）、11434（Hermes），仅局域网
     拒绝所有外部访问
出站：允许所有（HTTPS 443 用于 LLM API 调用）
```

---

## 网络诊断工具

### 命令行诊断

```bash
# 检查 IP 地址
ip a

# 检查路由表
ip route show

# 测试连通性
ping -c 4 192.168.1.1
ping -c 4 openrouter.ai

# DNS 解析测试
nslookup api.deepseek.com
```

---

## 网络故障排查

| 现象 | 排查步骤 |
|------|----------|
| 无法获取 IP | ① 检查网线是否插好 ② 重启路由器 DHCP 服务 ③ 尝试手动设置静态 IP |
| 无法调用 LLM API | ① `ping openrouter.ai` ② 检查企业防火墙 ③ 确认 API Key 有效 |
| 无法被局域网发现 | ① 确认同一子网 ② 关闭客户端防火墙 ③ 用 IP 直连 |
| 间歇性断网 | ① 更换网线 ② 更换路由器端口 ③ 检查交换机状态 |
