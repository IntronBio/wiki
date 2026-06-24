---
title: AI智能体能力
description: AIEdge 预装的三大AI智能体框架能力详解
---

# AI智能体能力

## 三大智能体框架

AIEdge 出厂预装三款核心 AI 智能体软件，全部通过 API 调用云端大模型，不消耗本地算力。

| 框架 | 定位 | 编程语言 | 核心场景 |
|------|------|:--:|------|
| **OpenClaw** | 多通道智能体网关 | Node.js 22+ | 消息平台接入、多代理管理 |
| **Hermes Agent** | 终端原生自治代理 | Python 3.10+ | 定时自动化、持久记忆 |
| **Claude Code** | AI编程助手 | Node.js | 代码生成、PR审查、Git集成 |

---

## OpenClaw — 多通道智能体网关

### 核心能力

- :material-message-text: **20+ 消息通道** — 企业微信 / 钉钉 / 飞书 / Slack / Telegram / Discord / WhatsApp / Signal / iMessage / Google Chat / Matrix ...
- :material-account-multiple: **多代理路由** — 不同联系人/群组/通道可路由到不同的智能体，每个智能体有独立的人格与技能配置
- :material-package-variant: **技能市场** — 13000+ 社区技能，按需安装
- :material-clock: **Cron 调度** — 定时任务、心跳监控、主动推送
- :material-database: **本地 SQLite** — 对话记忆、联系人信息全部本地存储
- :material-api: **Gateway API** — 开放 API 接口（`localhost:18789`），供外部系统调用

### 常用命令

```bash
# 安装并启动守护进程
openclaw onboard --install-daemon

# 安装消息通道插件
openclaw plugins install @openclaw/wechat-work
openclaw plugins install @openclaw/slack

# 安装技能
openclaw skill install weather-reporter
openclaw skill install github-pr-monitor

# 添加定时任务
openclaw cron add "0 9 * * *" "good morning briefing"

# 查看状态
openclaw status
openclaw logs --follow
```

---

## Hermes Agent — 终端原生自治代理

### 核心能力

- :material-brain: **持久化记忆** — 跨会话保留对话上下文，越用越聪明
- :material-auto-fix: **自进化技能** — 从成功经验中学习，自动生成新技能
- :material-message-outline: **21+ 消息网关** — 与 OpenClaw 互补的消息平台覆盖
- :material-swap-horizontal: **供应商无关** — 同一套配置可任意切换 Anthropic / OpenAI / OpenRouter / 通义千问 …
- :material-file-code: **本地技能文件** — 技能以 Markdown 文件定义，Git 版本控制友好

### 常用命令

```bash
# 初始化设置
hermes setup

# 启动守护进程
hermes start --daemon

# 配置 LLM 供应商
hermes config set provider openrouter
hermes config set model "anthropic/claude-sonnet-4-6"

# 添加自定义技能
hermes skill create "weekly-report" --description "生成周报"

# 查看记忆
hermes memory list --recent 10

# 运行一次性任务
hermes run "分析最近的客户反馈并生成报告"
```

---

## Claude Code — AI编程助手

### 核心能力

- :material-code-braces: **项目级代码理解** — 自动索引项目结构，跨文件上下文
- :material-git: **Git 原生集成** — 自动生成 Commit Message、Review PR、解决冲突
- :material-test-tube: **测试生成** — 读代码自动补全单元测试
- :material-console: **交互式REPL** — 终端内自由对话，即时修改代码
- :material-file-document: **文档生成** — 自动生成 API 文档、README

### 常用命令

```bash
# 交互式对话
claude

# 单次任务
claude "帮我重构 src/auth.py，提取公共逻辑"

# 审查当前改动
claude "Review the staged changes and suggest improvements"

# PR 审查模式
claude --pr-review "https://github.com/org/repo/pull/123"

# 自动修复模式
claude --fix "Fix all type errors in src/"
```

---

## 云端 LLM API 供应商兼容性

AIEdge 预装的所有智能体框架均支持以下 LLM 供应商：

| 供应商 | 代表模型 | API 格式 | 推荐场景 |
|--------|---------|:--:|------|
| Anthropic | Claude Opus / Sonnet / Haiku | Anthropic API | 代码、长文、复杂推理 |
| OpenAI | GPT-4o / GPT-4.1 | OpenAI API | 通用对话、创意 |
| OpenRouter | 300+ 模型统一入口 | OpenAI兼容 | 多模型切换、成本优化 |
| DeepSeek | DeepSeek-V3 / R1 | OpenAI兼容 | 高性价比推理 |
| 通义千问 | Qwen-Max / Qwen-Plus | OpenAI兼容 | 中文场景优化 |
| Google | Gemini 2.5 Pro | OpenAI兼容 | 多模态 |

!!! tip "推荐方案"
    使用 **OpenRouter** 作为统一入口（一个 API Key 访问所有模型），配合 Manifest 成本路由，可在保证质量的前提下节省 **30%-70%** 的 API 费用。
