---
title: 消息通道接入指南
description: AIEdge 智能体接入微信/钉钉/飞书/Slack 等消息平台详解
---

# 消息通道接入指南

AIEdge 预装的三大智能体框架各有不同的消息通道接入方式。以下以**微信**为例，分别介绍各框架的接入步骤。

---

## 一、OpenClaw 接入微信

OpenClaw（最新版 ≥ 2026.6.x）通过官方微信插件接入，扫码即可绑定。

### 1. 启用微信插件

```bash
# 安装微信官方插件
openclaw plugins install @tencent-weixin/openclaw-weixin --force

# 启用插件
openclaw config set plugins.entries.openclaw-weixin.enabled true

# 重启网关
openclaw gateway restart
```

### 2. 扫码绑定

```bash
# 调出微信登录二维码
openclaw channels login --channel openclaw-weixin
```

终端会输出二维码链接，浏览器打开后使用手机微信扫码 → 同意 ClawBot 授权即可完成绑定。

### 3. 多账号配置（可选）

```bash
# 多微信会话隔离
openclaw config set session.dmScope per-account-channel-peer
```

### 4. 排坑

| 问题 | 解决方案 |
|------|----------|
| 扫码无响应 | 执行 `openclaw gateway restart` 重启网关 |
| 旧版插件兼容 bug | 务必使用 2026.3 以上版本，已预装在 AIEdge 中 |

---

## 二、Hermes Agent 接入微信

Hermes Agent（v0.6.0+）原生支持腾讯 iLink，无需公网 IP，长轮询稳定。

### 1. 安装依赖

```bash
# 安装微信适配器依赖
pip install aiohttp cryptography qrcode
```

### 2. 配置微信通道

```bash
# 网关交互式配置向导
hermes setup gateway
```

操作步骤：
1. 列表选中 **Weixin**（空格勾选）→ 回车确认
2. 终端输出二维码链接，浏览器打开后微信扫码授权
3. 权限配置：私聊全开，群聊官方通道暂不稳定建议关闭
4. 保存配置，重启网关

```bash
hermes gateway restart
```

### 3. 关键限制

!!! warning "注意"
    - 仅稳定支持**私聊**，群消息易丢失
    - 同一微信号**不能同时**绑定 OpenClaw 和 Hermes，扫码会自动解绑旧绑定

---

## 三、Claude Code 接入微信

Claude Code 无原生微信通道，通过开源桥接工具 `claude-code-wechat-channel` 实现消息转发。

```
微信 ClawBot → 本地桥接服务 → Claude Code CLI
```

### 接入步骤

确保本地 `claude` 命令可正常调用（已配置 Anthropic API）。

```bash
# 初始化微信绑定
npx claude-code-wechat-channel setup
```

扫码授权微信 ClawBot，凭证保存在 `~/.claude/channels/wechat/account.json`。

```bash
# 安装 MCP 服务并后台常驻
npx claude-code-wechat-channel install

# 启动后台监听
bash ~/.claude/skills/claude-to-im/scripts/daemon.sh start
```

---

## 各框架微信接入对比

| 维度 | OpenClaw | Hermes Agent | Claude Code |
|------|:--------:|:------------:|:-----------:|
| 接入方式 | 官方插件 + 扫码 | 原生 iLink + 扫码 | 第三方桥接 |
| 公网 IP | 不需要 | 不需要 | 不需要 |
| 私聊 | ✅ | ✅ | ✅ |
| 群聊 | ✅ | ⚠️ 不稳定 | ⚠️ 受限 |
| 多账号 | ✅ | ❌ | ❌ |
| 推荐场景 | 全功能团队使用 | 个人私聊对话 | 代码场景问答 |

---

## 更多通道

完成微信接入后，可按相同模式接入其他平台（钉钉、飞书、Slack、Telegram 等），具体请参考各框架官方文档：

- OpenClaw: `openclaw plugins list` 查看可选插件
- Hermes Agent: `hermes setup gateway` 查看支持通道
- Claude Code: 社区桥接工具生态
