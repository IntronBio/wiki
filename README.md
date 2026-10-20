# AIEdge Wiki — 企业文档中心

基于 **MkDocs + Material for MkDocs** 搭建的微型AI服务器企业对外Wiki文档站点，风格对标 [LuatOS 官方文档站](https://wiki-zh.luatos.org/)。

## 目录结构

```
wiki/
├── mkdocs.yml              # MkDocs 全局配置文件
├── README.md               # 项目说明（本文件）
├── .gitignore              # Git 忽略规则
└── docs/                   # 文档源码目录
    ├── index.md            # 首页（Hero + 导航卡片 + 产品概述）
    ├── company.md          # 公司介绍
    ├── quickstart/         # 快速入门（设备简介/硬件参数/开箱/部署/启动）
    │   ├── index.md
    │   ├── overview.md
    │   ├── specs.md
    │   ├── unbox.md
    │   ├── deploy.md
    │   └── lan-start.md
    ├── manual/             # 设备使用手册（安装/开关机/配置/网络/运维）
    │   ├── index.md
    │   ├── install.md
    │   ├── power.md
    │   ├── config.md
    │   ├── network.md
    │   └── maintenance.md
    ├── features/           # 功能说明（推理能力/本地部署/适用场景）
    │   ├── index.md
    │   ├── inference.md
    │   ├── local-deploy.md
    │   └── scenarios.md
    ├── faq.md              # 常见问题 FAQ
    ├── resources.md        # 更多资料（参数表/下载/技术支持）
    ├── assets/             # 静态资源
    │   ├── logo.svg
    │   └── favicon.svg
    ├── stylesheets/
    │   └── extra.css       # 自定义样式
    ├── javascripts/
    │   └── extra.js        # 自定义脚本
    └── overrides/
        └── main.html       # 页脚自定义（去除第三方标识）
```

## 本地部署步骤

### 环境要求

- Python 3.8+
- pip

### 安装

```bash
# 1. 安装依赖（版本锁定，与线上构建一致）
pip install -r requirements.txt

# 2. 进入项目目录
cd wiki

# 3. 启动本地预览
mkdocs serve
```

浏览器访问：`http://localhost:8000`

### 局域网访问

```bash
# 启动时绑定所有接口，局域网内其他设备可访问
mkdocs serve --dev-addr 0.0.0.0:8000
```

局域网内其他设备访问：`http://[本机IP]:8000`

> 查看本机 IP：Linux/macOS 运行 `hostname -I` 或 `ifconfig`；Windows 运行 `ipconfig`

### 构建静态文件

```bash
# 构建到 site/ 目录
mkdocs build

# 构建产物可直接用任意 Web 服务器托管
# 例如使用 Nginx：
# server {
#     listen 80;
#     server_name localhost;
#     root /path/to/wiki/site;
#     index index.html;
# }
```

## Cloudflare Pages 部署

本项目可直接通过 Cloudflare Pages 导入 Git 仓库自动构建。

### 构建配置

| 配置项 | 值 |
|--------|-----|
| Framework preset | `None` |
| **Build command** | `pip install -r requirements.txt && mkdocs build` |
| **Build output directory** | `site` |
| Build image | v2（Python 默认 3.11.5，`.python-version` 已锁定 3.10） |
| 环境变量 | `SITE_URL=https://你的域名`（缺少时回退到 localhost） |

### 说明

- `requirements.txt` 锁定构建依赖版本；`.python-version` 指定 Python 3.10。
- `site_url` 通过 `SITE_URL` 环境变量注入（见 `mkdocs.yml`），用于生成 sitemap 和 canonical 链接，务必在 Cloudflare 项目设置中配置。
- 中文站点构建在根路径，英文站点构建在 `/en/` 下，无需额外路由配置。
- `site/` 是构建产物，已在 `.gitignore` 中忽略，无需提交。

### 手动上传（可选）

```bash
mkdocs build
npx wrangler pages deploy site --project-name=aiedge-wiki
```

## 主题特性

| 特性 | 说明 |
|------|------|
| 主题 | Material for MkDocs (Indigo 深蓝) |
| 布局 | 顶部导航 + 左侧目录 + 右侧锚点 + 主内容区 |
| 模式 | 浅色/深色一键切换 |
| 搜索 | 本地离线全文搜索（中文分词） |
| 代码 | 语法高亮 + 一键复制 |
| 组件 | 提示块、警告块、流程图(Mermaid)、表格、任务列表 |
| 响应式 | 电脑端/平板/手机完美适配 |
| 离线 | 纯本地运行，无联网依赖 |

## 导航结构

- 🏠 首页 — 网站概述、产品介绍、文档导航
- 🏢 公司介绍 — 企业概况、核心业务、技术优势
- 🚀 快速入门 — 设备简介、硬件参数、开箱、部署、启动
- 📖 设备使用手册 — 安装、开关机、配置、网络、运维
- ⚙️ 功能说明 — AI推理、本地部署、适用场景
- ❓ 常见问题 — 启动异常、网络故障、推理问题
- 📚 更多资料 — 参数表、SDK下载、技术支持
