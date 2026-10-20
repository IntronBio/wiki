---
title: 常见问题 FAQ
description: AIEdge 微型AI智能体服务器常见问题与故障排查
---

# ❓ 常见问题 FAQ

## 设备启动类

### Q1: 通电后蓝色指示灯不亮？

**排查步骤**：

1. 检查 Type-C 电源线是否插紧
2. 更换插座测试
3. 如以上正常但仍不亮，联系售后更换

---

### Q2: 开机后蓝色灯持续闪烁超过2分钟，不常亮？

**可能原因**：系统启动失败。

**解决方案**：

1. 拔掉电源，等待 30 秒
2. 重新插电启动
3. 若仍无法启动，长按复位按钮 10 秒强制重置
4. 联系技术支持获取系统恢复固件

---

### Q3: 如何恢复出厂设置？

命令行：`sudo aiedge-reset --factory`

!!! warning "注意"
    出厂重置将清除所有 API Key 配置、智能体记忆和数据。

---

## 网络连接类

### Q4: 无法通过 `aiedge.local` 域名访问？

**原因**：Windows 系统默认不支持 mDNS。

**解决方案**：使用 IP 地址直接 SSH 登录；在路由器管理后台查找设备 IP（主机名为 `aiedge`），或连接 HDMI 显示器登录后执行 `ip a` 查看 eth0 地址。

---

### Q5: 设备能启动但无法获取 IP 地址？

**排查步骤**：

1. 检查网线是否正确插入
2. 检查路由器 DHCP 服务是否开启
3. 更换网线测试
4. 尝试手动设置静态 IP

---

### Q6: 智能体无法调用云端 API？

**排查步骤**：

1. 确认设备能访问互联网：`ping openrouter.ai`
2. 检查 API Key 是否配置：`aiedge config list-api-keys`
3. 检查 API Key 是否有效（余额/限额）
4. 检查企业防火墙是否拦截 HTTPS 出站
5. 查看智能体日志：`tail -f /var/log/openclaw/gateway.log`

---

## 智能体使用类

### Q7: OpenClaw 启动失败？

**常见原因**：

| 原因 | 解决方案 |
|------|----------|
| Node.js 版本过低 | `node --version` 确认 ≥ 22 |
| 端口被占用 | `sudo lsof -i :18789` 检查 |
| API Key 未配置 | `openclaw config list` 检查 |

```bash
# 查看详细错误
journalctl -u openclaw -n 50
```

---

### Q8: Hermes Agent 无响应？

**排查步骤**：

1. 检查服务状态：`sudo systemctl status hermes-agent`
2. 检查 API Key 配置：`hermes config show`
3. 重启服务：`sudo systemctl restart hermes-agent`
4. 查看日志：`journalctl -u hermes-agent -n 50`

---

### Q9: 消息通道收不到智能体回复？

**排查步骤**：

1. 确认消息通道插件已安装：`openclaw plugins list`
2. 确认通道配置正确（Webhook URL、Token等）
3. 检查通道是否在线：`openclaw channels status`
4. 测试 API 连通性：发送测试消息

---

### Q10: 内存不足怎么办？

**解决方案**：

1. 关闭不使用的消息通道插件
2. 减少同时活跃的智能体数量
3. 定期清理日志文件
4. 设置定时重启
5. 升级至 4GB 版本

---

## 系统运维类

### Q11: 设备运行久了变慢？

**解决方案**：

1. 设置每周定时重启（推荐凌晨）
2. 清理旧日志释放磁盘
3. 检查是否开启了过多消息通道

---

### Q12: 如何升级智能体软件？

```bash
# 升级 OpenClaw
npm update -g openclaw

# 升级 Hermes
pip install --upgrade hermes-agent

# 升级 Claude Code
npm update -g @anthropic-ai/claude-code
```

---

### Q13: 忘记 root 密码怎么办？

1. 长按设备复位按钮 30 秒恢复出厂设置
2. 使用默认账户登录：`root` / `hybio.tech`（密码输入时不显示，盲输后回车）
3. 重新配置 API Key

!!! warning "注意"
    恢复出厂将清除所有数据。

---

## 采购与售后类

### Q14: 1GB / 2GB / 4GB 版本如何选择？

| 场景 | 推荐 |
|------|------|
| 个人开发/学习 | 1GB |
| 小团队客服/自动化 | 2GB |
| 部门级多智能体 | 4GB |
| 大型多场景部署 | 多台 4GB + 分区 |

---

### Q15: 售后服务如何联系？

- :material-email: 邮件：info@hybio.tech
- :material-phone: 技术支持：19262160086（微信同号）
- :material-phone: 联系电话：17501621170
- :material-clock: 工作时间：周一至周五 9:00-18:00
- 紧急故障：4 小时内响应

---

## 仍未解决？

请联系技术支持，准备好以下信息：

- 设备序列号（底部标签）
- Ubuntu 版本：`lsb_release -a`
- 智能体版本：`openclaw --version && hermes --version && claude --version`
- 故障现象描述（含截图/日志）
- 已尝试的排查步骤
