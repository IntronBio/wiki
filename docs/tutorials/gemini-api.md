---
title: 配置 Gemini API
description: Ubuntu SSH 环境下配置 Claude Code / OpenClaw / Hermes 三套工具的 Gemini API
---

# 🔑 配置 Gemini API

在 AIEdge 上为 Claude Code、OpenClaw、Hermes Agent 三套工具配置 Google Gemini API。

!!! tip "前置准备"
    先去 [aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey) 创建 Gemini API Key，格式为 `AIzaSy...`。

---

## 一、OpenClaw（原生支持，最简单）

OpenClaw 原生支持 Gemini API，三种配置方式任选其一。

### 方式 1：交互式命令（推荐）

```bash
openclaw onboard --auth-choice gemini-api-key
```

按提示粘贴你的 `AIzaSy...` 密钥，自动写入配置文件。

### 方式 2：环境变量写入 `~/.bashrc`（永久生效）

```bash
# 追加全局变量
cat >> ~/.bashrc <<'EOF'
# OpenClaw Gemini
export GEMINI_API_KEY="AIzaSy你的完整密钥"
export GOOGLE_API_KEY="AIzaSy你的完整密钥"
EOF
source ~/.bashrc
```

### 方式 3：手动编辑配置文件

```bash
nano ~/.openclaw/openclaw.json
```

```json
{
  "models": {
    "providers": {
      "google": {
        "apiKey": "AIzaSy你的密钥"
      }
    },
    "defaults": {
      "model": {
        "primary": "google/gemini-3.1-pro-preview"
      }
    }
  }
}
```

保存后重启：

```bash
openclaw gateway restart
```

### 支持模型

`gemini-3.1-pro-preview` / `gemini-2.5-flash` / `gemini-2.5-pro`

### 验证

```bash
openclaw model list
```

能看到 `google/gemini` 系列即成功。

---

## 二、Hermes Agent

Hermes 原生支持 Gemini，通过 `.env` 文件配置。

### 1. 写入密钥

```bash
# 创建/编辑 ~/.hermes/.env
echo "GOOGLE_API_KEY=AIzaSy你的密钥" >> ~/.hermes/.env
echo "GEMINI_API_KEY=AIzaSy你的密钥" >> ~/.hermes/.env
```

### 2. 切换默认模型

```bash
hermes model
# 选择 More providers → Google AI Studio
# 推荐：gemini-3-flash（快速）/ gemini-3-pro（强推理）
```

### 3. 手动固定默认模型

编辑 `~/.hermes/config.yaml`：

```yaml
model:
  provider: gemini
  default: gemini-3.1-pro-preview
```

### 验证

```bash
hermes test
```

返回 Gemini 输出即正常。

---

## 三、Claude Code

!!! warning "注意"
    Claude Code 只认 Anthropic 协议接口，Google Gemini 原生接口不兼容，不能直接填 `generativelanguage.googleapis.com`（会 404）。需要走代理转换。

### 方案 A：本地中转代理（推荐，纯本机无依赖）

用 `gemini-claude-code` 一键启动翻译代理，把 Gemini 转成 Anthropic 格式。

```bash
# 安装代理工具
npm install -g gemini-claude-code

# 后台启动代理（端口 8080）
gemini-claude-code --api-key AIzaSy你的密钥 --port 8080 &

# 临时生效（当前 SSH）
export ANTHROPIC_BASE_URL="http://127.0.0.1:8080"
export ANTHROPIC_AUTH_TOKEN="dummy"
export ANTHROPIC_MODEL="gemini-3.1-pro-preview"
claude
```

**永久写入 bashrc，开机自动加载：**

```bash
cat >> ~/.bashrc <<'EOF'
# Gemini Proxy for Claude Code
gemini-claude-code --api-key AIzaSy你的密钥 --port 8080 >/dev/null 2>&1 &
export ANTHROPIC_BASE_URL="http://127.0.0.1:8080"
export ANTHROPIC_AUTH_TOKEN="dummy"
export ANTHROPIC_MODEL="gemini-3.1-pro-preview"
EOF
source ~/.bashrc
```

### 方案 B：通过 OpenRouter 中转

去 [OpenRouter](https://openrouter.ai/) 绑定 Gemini API，获取 `sk-or-v1-xxx` 密钥，配置逻辑和 DeepSeek 一致：

```bash
cat >> ~/.bashrc <<'EOF'
export ANTHROPIC_BASE_URL="https://openrouter.ai/api/anthropic"
export ANTHROPIC_AUTH_TOKEN="sk-or-v1-你的OpenRouter密钥"
export ANTHROPIC_MODEL="google/gemini-3.1-pro-preview"
EOF
source ~/.bashrc
claude /status
```

---

## 一键切换别名（DeepSeek / Gemini）

将以下内容写入 `~/.bashrc`，之后用 `cc-ds` / `cc-gem` 快速切换：

```bash
cat >> ~/.bashrc <<'EOF'
# Claude Code 快速切换命令
cc-ds() {
  export ANTHROPIC_BASE_URL="https://api.deepseek.com/anthropic"
  export ANTHROPIC_AUTH_TOKEN="sk-你的DeepSeek密钥"
  export ANTHROPIC_MODEL="deepseek-v4-pro[1m]"
  claude "$@"
}
cc-gem() {
  export ANTHROPIC_BASE_URL="http://127.0.0.1:8080"
  export ANTHROPIC_AUTH_TOKEN="dummy"
  export ANTHROPIC_MODEL="gemini-3.1-pro-preview"
  claude "$@"
}
EOF
source ~/.bashrc
```

使用：

```bash
cc-ds "帮我分析代码"    # 走 DeepSeek
cc-gem "帮我分析代码"   # 走 Gemini
```
