---
title: Configure Gemini API
description: Configure Google Gemini API for Claude Code / OpenClaw / Hermes in Linux SSH environment
---

# 🔑 Configure Gemini API

Set up Google Gemini API for Claude Code, OpenClaw, and Hermes Agent on your AIEdge.

!!! tip "Prerequisites"
    First, go to [aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey) to create a Gemini API Key — it will be in the format `AIzaSy...`.

---

## 1. OpenClaw (Native Support — Easiest)

OpenClaw natively supports the Gemini API. Choose from three configuration methods.

### Method 1: Interactive Command (Recommended)

```bash
openclaw onboard --auth-choice gemini-api-key
```

Follow the prompts and paste your `AIzaSy...` key — it will be automatically written to the config file.

### Method 2: Environment Variables in `~/.bashrc` (Permanent)

```bash
# Append global variables
cat >> ~/.bashrc <<'EOF'
# OpenClaw Gemini
export GEMINI_API_KEY="AIzaSyYourFullKeyHere"
export GOOGLE_API_KEY="AIzaSyYourFullKeyHere"
EOF
source ~/.bashrc
```

### Method 3: Manual Config File Edit

```bash
nano ~/.openclaw/openclaw.json
```

```json
{
  "models": {
    "providers": {
      "google": {
        "apiKey": "AIzaSyYourKeyHere"
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

Save and restart:

```bash
openclaw gateway restart
```

### Supported Models

`gemini-3.1-pro-preview` / `gemini-2.5-flash` / `gemini-2.5-pro`

### Verify

```bash
openclaw model list
```

If you can see the `google/gemini` series, it's working.

---

## 2. Hermes Agent

Hermes natively supports Gemini — configure via `.env` file.

### 1. Write the Key

```bash
# Create/edit ~/.hermes/.env
echo "GOOGLE_API_KEY=AIzaSyYourKeyHere" >> ~/.hermes/.env
echo "GEMINI_API_KEY=AIzaSyYourKeyHere" >> ~/.hermes/.env
```

### 2. Switch the Default Model

```bash
hermes model
# Select More providers → Google AI Studio
# Recommended: gemini-3-flash (fast) / gemini-3-pro (strong reasoning)
```

### 3. Manually Pin the Default Model

Edit `~/.hermes/config.yaml`:

```yaml
model:
  provider: gemini
  default: gemini-3.1-pro-preview
```

### Verify

```bash
hermes test
```

If you get a Gemini response, it's working.

---

## 3. Claude Code

!!! warning "Note"
    Claude Code only speaks the Anthropic API protocol. Google Gemini's native API is incompatible — pointing it directly at `generativelanguage.googleapis.com` will result in a 404. You need a translation proxy.

### Option A: Local Translation Proxy (Recommended — No External Dependencies)

Use `gemini-claude-code` to spin up a one-click translation proxy that converts Gemini to Anthropic format.

```bash
# Install the proxy tool
npm install -g gemini-claude-code

# Start the proxy in the background (port 8080)
gemini-claude-code --api-key AIzaSyYourKeyHere --port 8080 &

# Temporary (current SSH session only)
export ANTHROPIC_BASE_URL="http://127.0.0.1:8080"
export ANTHROPIC_AUTH_TOKEN="dummy"
export ANTHROPIC_MODEL="gemini-3.1-pro-preview"
claude
```

**Permanently write to bashrc, auto-load on boot:**

```bash
cat >> ~/.bashrc <<'EOF'
# Gemini Proxy for Claude Code
gemini-claude-code --api-key AIzaSyYourKeyHere --port 8080 >/dev/null 2>&1 &
export ANTHROPIC_BASE_URL="http://127.0.0.1:8080"
export ANTHROPIC_AUTH_TOKEN="dummy"
export ANTHROPIC_MODEL="gemini-3.1-pro-preview"
EOF
source ~/.bashrc
```

### Option B: Route Through OpenRouter

Go to [OpenRouter](https://openrouter.ai/), bind your Gemini API, and get a `sk-or-v1-xxx` key. The configuration is the same as for DeepSeek:

```bash
cat >> ~/.bashrc <<'EOF'
export ANTHROPIC_BASE_URL="https://openrouter.ai/api/anthropic"
export ANTHROPIC_AUTH_TOKEN="sk-or-v1-YourOpenRouterKeyHere"
export ANTHROPIC_MODEL="google/gemini-3.1-pro-preview"
EOF
source ~/.bashrc
claude /status
```

---

## One-Command Switch Aliases (DeepSeek / Gemini)

Add the following to `~/.bashrc`, then use `cc-ds` / `cc-gem` to quickly switch:

```bash
cat >> ~/.bashrc <<'EOF'
# Claude Code quick-switch aliases
cc-ds() {
  export ANTHROPIC_BASE_URL="https://api.deepseek.com/anthropic"
  export ANTHROPIC_AUTH_TOKEN="sk-YourDeepSeekKeyHere"
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

Usage:

```bash
cc-ds "Analyze this code"    # Uses DeepSeek
cc-gem "Analyze this code"   # Uses Gemini
```
