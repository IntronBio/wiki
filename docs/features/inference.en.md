---
title: AI Agent Capabilities
description: AIEdge Pre-Installed and Optional AI Agent Frameworks — Detailed Capabilities
---

# AI Agent Capabilities

## Pre-Installed & Optional Frameworks

AIEdge ships with **OpenClaw** and **Codex CLI** pre-installed; Mihomo (Clash), Hermes Agent, and Claude Code can be installed on demand. All frameworks call cloud LLMs via API — no local compute consumed.

| Framework | Role | Language | Core Scenarios | Installation |
|------|------|:--:|------|:--:|
| **OpenClaw** | Multi-channel agent gateway | Node.js 22+ | Messaging platform integration, multi-agent management | Pre-installed |
| **Codex CLI** | OpenAI terminal coding agent | Node.js | Code generation, terminal automation, Git integration | Pre-installed |
| **Hermes Agent** | Native terminal autonomous agent | Python 3.10+ | Scheduled automation, persistent memory | Optional |
| **Claude Code** | AI coding assistant | Node.js | Code generation, PR review, Git integration | Optional |

---

## OpenClaw — Multi-Channel Agent Gateway

### Core Capabilities

- :material-message-text: **20+ Messaging Channels** — WeChat Work / DingTalk / Lark / Slack / Telegram / Discord / WhatsApp / Signal / iMessage / Google Chat / Matrix ...
- :material-account-multiple: **Multi-Agent Routing** — Different contacts/groups/channels can be routed to different agents, each with independent personality and skill configurations
- :material-package-variant: **Skill Marketplace** — 13,000+ community skills, install as needed
- :material-clock: **Cron Scheduling** — Scheduled tasks, heartbeat monitoring, proactive push
- :material-database: **Local SQLite** — Conversation memory and contact info stored locally
- :material-api: **Gateway API** — Open API interface (`localhost:18789`) for external system integration

### Common Commands

```bash
# Install and start daemon
openclaw onboard --install-daemon

# Install messaging channel plugins
openclaw plugins install @openclaw/wechat-work
openclaw plugins install @openclaw/slack

# Install skills
openclaw skill install weather-reporter
openclaw skill install github-pr-monitor

# Add cron job
openclaw cron add "0 9 * * *" "good morning briefing"

# Check status
openclaw status
openclaw logs --follow
```

---

## Codex CLI — OpenAI Terminal Coding Agent

### Core Capabilities

- :material-console: **Terminal-native** — Complete coding tasks conversationally right from your SSH terminal
- :material-code-braces: **Project-level understanding** — Reads repository context to generate, modify, and refactor code
- :material-git: **Git workflow** — Inspect changes, generate commit messages, assist with code review
- :material-cloud: **Cloud models** — Calls cloud models via a ChatGPT account or OpenAI API key

### Common Commands

```bash
# Start an interactive coding session
codex

# Run a task non-interactively
codex exec "Add a README for this project"

# Sign in / switch account
codex login
```

---

## Hermes Agent (Optional) — Terminal Native Autonomous Agent

### Core Capabilities

- :material-brain: **Persistent Memory** — Retains conversation context across sessions, getting smarter over time
- :material-auto-fix: **Self-Evolving Skills** — Learns from successful experiences, auto-generates new skills
- :material-message-outline: **21+ Messaging Gateway** — Complementary messaging platform coverage to OpenClaw
- :material-swap-horizontal: **Provider-Agnostic** — Same config, switch between Anthropic / OpenAI / OpenRouter / Qwen ...
- :material-file-code: **Local Skill Files** — Skills defined as Markdown files, Git version-control friendly

### Common Commands

```bash
# Initial setup
hermes setup

# Start daemon
hermes start --daemon

# Configure LLM provider
hermes config set provider openrouter
hermes config set model "anthropic/claude-sonnet-4-6"

# Add custom skill
hermes skill create "weekly-report" --description "Generate weekly report"

# View memory
hermes memory list --recent 10

# Run one-shot task
hermes run "Analyze recent customer feedback and generate a report"
```

---

## Claude Code (Optional) — AI Coding Assistant

### Core Capabilities

- :material-code-braces: **Project-Level Code Understanding** — Auto-indexes project structure, cross-file context
- :material-git: **Native Git Integration** — Auto-generate commit messages, review PRs, resolve conflicts
- :material-test-tube: **Test Generation** — Reads code and auto-completes unit tests
- :material-console: **Interactive REPL** — Free-form conversation in terminal, instant code modifications
- :material-file-document: **Documentation Generation** — Auto-generate API docs, READMEs

### Common Commands

```bash
# Interactive mode
claude

# One-shot task
claude "Refactor src/auth.py, extract common logic"

# Review staged changes
claude "Review the staged changes and suggest improvements"

# PR review mode
claude --pr-review "https://github.com/org/repo/pull/123"

# Auto-fix mode
claude --fix "Fix all type errors in src/"
```

---

## Cloud LLM API Provider Compatibility

All frameworks above support the following LLM providers:

| Provider | Representative Models | API Format | Recommended For |
|--------|---------|:--:|------|
| Anthropic | Claude Opus / Sonnet / Haiku | Anthropic API | Code, long-form text, complex reasoning |
| OpenAI | GPT-4o / GPT-4.1 | OpenAI API | General conversation, creative work |
| OpenRouter | 300+ models, unified access | OpenAI-compatible | Multi-model switching, cost optimization |
| DeepSeek | DeepSeek-V3 / R1 | OpenAI-compatible | Cost-effective reasoning |
| Qwen | Qwen-Max / Qwen-Plus | OpenAI-compatible | Chinese-language scenarios |
| Google | Gemini 2.5 Pro | OpenAI-compatible | Multimodal |

!!! tip "Recommended Approach"
    Use **OpenRouter** as a unified entry point (one API Key for all models), combined with Manifest cost routing — save **30%-70%** on API costs while maintaining quality.
