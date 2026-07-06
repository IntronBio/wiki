---
title: Claude Code + DeepSeek 设计部署网页
description: 用 AIEdge 上的 Claude Code 配合 DeepSeek API，从零设计并部署一个网页
---

# 🌐 Claude Code + DeepSeek：设计并部署一个网页

本教程演示如何在 AIEdge 上使用 **Claude Code** 搭配 **DeepSeek API**，从零开始设计一个网页，并部署到局域网中供团队访问。

---

## 第一步：配置 DeepSeek API Key（永久生效）

SSH 登录 AIEdge 后，将 DeepSeek API 配置写入 `~/.bashrc`，一次配置，永久生效。

### 1. 一键追加配置

替换 `sk-` 开头的密钥为你自己的 DeepSeek API Key，然后复制整段执行：

```bash
cat >> ~/.bashrc <<'EOF'
# Claude Code 对接 DeepSeek API 全局配置
export ANTHROPIC_BASE_URL="https://api.deepseek.com/anthropic"
export ANTHROPIC_AUTH_TOKEN="sk-这里填你的DeepSeek完整密钥"
export ANTHROPIC_MODEL="deepseek-v4-pro[1m]"
export ANTHROPIC_DEFAULT_OPUS_MODEL="deepseek-v4-pro[1m]"
export ANTHROPIC_DEFAULT_SONNET_MODEL="deepseek-v4-pro"
export ANTHROPIC_DEFAULT_HAIKU_MODEL="deepseek-v4-flash"
export CLAUDE_CODE_SUBAGENT_MODEL="deepseek-v4-flash"
EOF
```

### 2. 重载配置，立刻生效

```bash
source ~/.bashrc
```

### 3. 验证配置

```bash
# 查看环境变量是否加载
echo $ANTHROPIC_BASE_URL

# 启动 Claude 测试
claude /status
```

![获取 DeepSeek API Key](../assets/web/获取ds-api.png){: style="max-width:600px;"}

!!! tip "获取 DeepSeek API Key"
    前往 [platform.deepseek.com](https://platform.deepseek.com) 注册并获取 API Key。DeepSeek 按量计费，价格低廉，充值少量金额即可使用很久。
    
    **推荐使用 SSH 方式**：写入 `~/.bashrc` 后，每次 SSH 登录自动加载，无需重复配置。

---

## 第二步：创建项目目录

```bash
mkdir -p /root/projects/my-website
cd /root/projects/my-website
```

---

## 第三步：用 Claude Code 生成网页

在 AIEdge 上启动 Claude Code，向它描述你想要设计的网页：

```bash
claude "帮我设计一个简洁的企业官网首页，包含以下内容：
- 顶部导航栏（公司名 + 关于我们 / 产品 / 联系方式）
- Hero 区域（大标题 + 副标题 + CTA 按钮）
- 三列功能卡片
- 页脚版权信息
- 使用纯 HTML + CSS，不依赖任何框架
- 响应式设计，手机也能看"
```

![键入提示词](../assets/web/键入提示词.png){: style="max-width:600px;"}

Claude Code 接收需求后开始思考与编码：

![Claude 思考中](../assets/web/thinking.png){: style="max-width:600px;"}

任务完成，Claude Code 给出生成报告。与此同时，它已经自动把网页部署到了本地 HTTP 服务上——无需手动操作。

![任务完成报告](../assets/web/claude任务完成报告.png){: style="max-width:600px;"}

---

## 第四步：局域网预览与调整

在同一局域网内，任意设备浏览器访问 AIEdge 的 IP + 端口即可看到网页：

```
http://192.168.1.xxx:8080
```

手机、平板、同事电脑都能打开——无需云服务器，无需域名备案。

![生成的网页](../assets/web/网页截图.png){: style="max-width:600px;"}

如果对效果不满意，回到 SSH 继续对话微调。比如换个背景：

```bash
claude "换个简洁的背景，整体风格清爽一点"
```

![调整后的网页](../assets/web/换背景之后的网页截图.png){: style="max-width:600px;"}

反复迭代直到满意。

---

## 第五步：发布到公网（可选）

局域网只有内网能访问。如果想**让外网也能看到**，推荐使用 **Cloudflare Tunnel**，免费、无需公网 IP、自带 HTTPS。

我们在公众号写过一篇完整的配置教程：

[:material-open-in-new: Cloudflare Tunnel 免费内网穿透教程](https://mp.weixin.qq.com/s/5zSdqS9lWovrzmBgeZOEyQ){ .md-button target="_blank"}

配置完成后，外网用户即可通过你的域名访问 AIEdge 上的网页。我们自己的 Demo 就挂在：

> 🔗 [https://demo.hybio.tech/](https://demo.hybio.tech/)

---

## 第六步：设置开机自启（可选）

让网页随设备启动自动运行：

```bash
# 创建 systemd 服务
cat > /etc/systemd/system/my-website.service << EOF
[Unit]
Description=My Website
After=network.target

[Service]
Type=simple
WorkingDirectory=/root/projects/my-website
ExecStart=python3 -m http.server 8080 --bind 0.0.0.0
Restart=always

[Install]
WantedBy=multi-user.target
EOF

# 启用并启动
systemctl enable my-website
systemctl start my-website
```

---

## 小结

| 环节 | 耗时 |
|------|:----:|
| 配置 API Key | 2 分钟 |
| 生成网页 | 3 分钟 |
| 微调迭代 | 5 分钟 |
| 内网预览 | 即时 |
| 公网发布（可选） | 10 分钟 |
| **总计** | **约 20 分钟** |

一台巴掌大的 AIEdge，配上一个 API Key，十分钟让想法变成局域网里人人能访问的网页，再来十分钟配上 Cloudflare Tunnel 让全世界都能看到。这就是 AIEdge 的日常。

---

## 下一步

返回 **[生产力教程首页](index.md)** 查看更多实战示例。
