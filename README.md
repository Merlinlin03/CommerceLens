# CommerceLens

> **电商智能问数与归因分析平台** —— 让业务人员用自然语言直接问数据、自动定位波动根因。

[![Python](https://img.shields.io/badge/Python-3.11+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![Vue](https://img.shields.io/badge/Vue-3-4FC08D?logo=vue.js&logoColor=white)](https://vuejs.org/)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white)](https://docs.docker.com/compose/)

---

## 📖 项目简介

**CommerceLens** 是一个面向电商场景的智能问数与归因分析平台。基于 **大语言模型（LLM）+ NL2SQL + 多 Agent 协作** 技术，业务人员无需掌握 SQL，即可通过自然语言对话查询电商数据，并自动完成多维归因分析，快速定位 GMV、转化率、客单价等核心指标波动的根本原因。

平台采用 **Planner / Explorer / Analyst / Reviewer** 多 Agent 协作架构，结合隔离沙箱执行、语义索引召回与只读 SQL 安全校验，在保证数据安全的前提下，实现从"提问 → 查数 → 归因 → 结论"的全链路自动化。

---

## ✨ 核心特性

| 特性 | 说明 |
| --- | --- |
| 🗣️ **自然语言问数** | 支持多轮对话式查询，LLM 自动生成只读 SQL 并返回可视化结果 |
| 🔍 **智能归因分析** | 多 Agent 协作，从维度拆解、指标下钻到根因定位，输出可解释的归因结论 |
| 🛡️ **企业级数据安全** | 只读 SQL 校验、列级 / 行级权限控制、Doris 账号隔离、Docker 沙箱执行 |
| 🧠 **语义元数据召回** | 表 / 字段 / 指标元数据管理，Elasticsearch 语义索引 + 取值索引双路召回 |
| 🔄 **跨存储工作流** | 支持跨 Doris / PostgreSQL / Elasticsearch 的长期复合任务与资源级联清理 |
| 📊 **完整可观测性** | 请求跟踪、统一错误响应、后台任务状态查询、查询经验沉淀 |

---

## 🏗️ 系统架构
┌─────────────────────────────────────────────────────────────┐
│                      前端 (Vue 3 + Vite)                     │
│                   对话问数 · 归因报告 · 管理中心              │
└────────────────────────────┬────────────────────────────────┘
                             │ HTTP / SSE
┌────────────────────────────▼────────────────────────────────┐
│                   后端 (FastAPI + Celery)                    │
│  ┌──────────┬──────────┬──────────┬──────────┬──────────┐   │
│  │ Identity │ Metadata │  Query   │Assistant │Workflows │   │
│  │ 认证授权 │ 语义召回 │ 安全查询 │ 多Agent  │ 跨存储   │   │
│  └──────────┴──────────┴──────────┴──────────┴──────────┘   │
│  ┌──────────────────────────────────────────────────────┐   │
│  │              Shared 基础能力层                        │   │
│  └──────────────────────────────────────────────────────┘   │
└───────┬──────────────┬──────────────┬──────────────┬────────┘
        │              │              │              │
   ┌────▼────┐   ┌────▼────┐   ┌────▼────┐   ┌────▼────┐
   │  Doris  │   │PostgreSQL│   │  Redis  │   │   ES    │
   │ 数据仓库│   │ 元数据/  │   │ 缓存/   │   │ 语义    │
   │         │   │ 认证/LG │   │ 队列    │   │ 索引    │
   └─────────┘   └─────────┘   └─────────┘   └─────────┘
                             │
                    ┌────────▼────────┐
                    │  Sandbox 沙箱   │
                    │ (Docker 隔离)   │
                    └─────────────────┘
---

## 🧩 模块文档

详细的架构设计与各模块实现文档位于 [`docs/`](docs/) 目录：

| 模块 | 文档 | 说明 |
| --- | --- | --- |
| 架构总览 | [00. 架构与协作总览](docs/00_ARCHITECTURE_OVERVIEW.md) | 系统总体架构、数据流向与多 Agent 协作机制 |
| Shared | [01. Shared 基础能力](docs/01_SHARED.md) | 数据库与外部服务连接、公共数据格式、错误响应、请求跟踪与后台任务 |
| Identity | [02. Identity 认证与授权](docs/02_IDENTITY.md) | 用户认证、Token 管理、Doris 账号与查询权限控制（列级与行级） |
| Metadata | [03. Metadata 元数据与语义召回](docs/03_METADATA.md) | 表/字段/指标元数据管理、Elasticsearch 语义与取值索引 |
| Sandbox | [04. Sandbox 隔离工作区](docs/04_SANDBOX.md) | Docker 容器隔离执行环境、文件权限与多进程沙箱协调 |
| Query | [05. Query 安全查询链路](docs/05_QUERY.md) | 只读 SQL 校验与安全执行、结果导出 CSV 及查询经验沉淀 |
| Assistant | [06. Assistant 多 Agent 分析体系](docs/06_ASSISTANT.md) | Planner / Explorer / Analyst / Reviewer 调度、会话管理与流式推送 |
| Workflows | [07. Workflows 跨存储工作流](docs/07_WORKFLOWS.md) | 跨多存储与环境的长期复合任务（用户注销与资源级联清理） |

---

## 🚀 快速开始

### 环境要求

- Python 3.11+
- Node.js 18+
- Docker & Docker Compose
- [uv](https://docs.astral.sh/uv/)（Python 包管理器）
- Git LFS（用于拉取全量示例数据）

## 配置

### 后端环境变量

复制环境变量模板：

```bash
cp conf/.env.example conf/.env
```

编辑 `conf/.env`：

```dotenv
# 本地 docker/compose.yml 的默认密码均为
DORIS_ADMIN_PASSWORD=
POSTGRES_PASSWORD=

# 分别执行下方命令生成
DORIS_CREDENTIAL_ENCRYPTION_KEY=
JWT_SECRET=

# 外部服务密钥
TAVILY_API_KEY=
SILICONFLOW_API_KEY=
OPENROUTER_API_KEY=
DEEPSEEK_API_KEY=

# 首次创建的平台管理员
ADMIN_USERNAME=admin
ADMIN_EMAIL=admin@example.com
ADMIN_PASSWORD=
```

生成 Doris 凭据加密密钥和 JWT 密钥：

```bash
python3 -c 'import base64, secrets; print(base64.urlsafe_b64encode(secrets.token_bytes(32)).decode())'
python3 -c 'import secrets; print(secrets.token_urlsafe(48))'
```

将两条命令的输出分别复制到 `DORIS_CREDENTIAL_ENCRYPTION_KEY` 和 `JWT_SECRET`。

当前 `conf/app_config.yaml` 引用了以下外部服务环境变量，保留对应配置时需要填写有效密钥：

| 环境变量              | 用途                                       |
| --------------------- | ------------------------------------------ |
| `DEEPSEEK_API_KEY`    | 默认语言模型 `deepseek-deepseek-v4-flash`  |
| `OPENROUTER_API_KEY`  | `app_config.yaml` 中声明的 OpenRouter 模型 |
| `SILICONFLOW_API_KEY` | `BAAI/bge-m3` 文本向量模型                 |
| `TAVILY_API_KEY`      | Tavily MCP 搜索工具                        |

### 应用配置

应用运行参数位于 `conf/app_config.yaml`。本地使用 `docker/compose.yml` 时可直接采用默认配置。连接已有服务或部署多个实例时，重点调整：

- `doris`、`auth_postgresql`、`meta_postgresql`、`langgraph_postgresql`、`elasticsearch`：服务地址、端口、账号和数据库。
- `task_queue`、`auth.rate_limit_redis_url`、`sandbox.ownership.redis_url`：Redis 连接地址。
- `lm_config.active` 与 `lm_config.models`：默认模型及模型服务参数。
- `embedding`：向量模型地址、模型名和向量维度；`elasticsearch.embedding_size` 必须与模型输出维度一致。
- `mcp`：Explorer 可用的 MCP 服务。删除不使用的服务配置后，无需提供对应密钥。
- `sandbox.image`：必须与 Compose 构建的沙箱镜像名一致。
- `sandbox.deployment_namespace`：同一 Docker 主机上的每套部署使用不同值。
- `cors_origins`：前后端跨域部署时加入前端 Origin，例如 `http://localhost:7001`。

### 前端代理

复制前端环境变量模板：

```bash
cp web/.env.example web/.env
```

`web/.env` 默认将 `/api` 代理到本机后端：

```dotenv
VITE_APP_PROXY=http://localhost:7000
```

后端地址变化时修改该值。

## 启动

### 1. 安装依赖

```bash
uv sync
npm --prefix web ci
```

### 2. 启动基础服务

```bash
docker compose -f docker/compose.yml up -d
```

该命令启动 PostgreSQL、Elasticsearch、Redis 和 Doris，并在缺少 `dataagent-sandbox:latest` 时构建沙箱镜像。PostgreSQL 的 `auth`、`meta` 和 `langgraph` 数据库会在首次创建数据卷时自动初始化。

查看服务状态：

```bash
docker compose -f docker/compose.yml ps
```

### 3. 准备 Doris 全量数据

默认应用连接 Doris 的 `ecommerce` 数据库。全量数据依赖 Git LFS 中的数据文件，先在项目根目录拉取：

```bash
git lfs install
git lfs pull
```

创建 `dbmock` 配置并生成两年全量数据：

```bash
cp dbmock/.env.example dbmock/.env
# 将 dbmock/.env 中的 DB_PASSWORD 设置为 123123

cd dbmock
uv sync
uv run scripts/init_db.py
uv run main.py
cd ..
```

`dbmock/scripts/init_db.py` 会删除并重建 `DB_NAME` 指定的数据库，只能用于可重建的本地数据。全量数据生成通常需要十几分钟，实际耗时取决于本机资源和 Doris 负载。连接已有 Doris 时跳过本步骤，并在 `conf/app_config.yaml` 中填写对应连接信息。

### 4. 创建管理员

```bash
uv run -m scripts.bootstrap_admin
```

该命令读取 `conf/.env` 中的 `ADMIN_USERNAME`、`ADMIN_EMAIL` 和 `ADMIN_PASSWORD`，可重复执行。

### 5. 启动应用

在四个项目根目录终端中分别启动后端、前端、Celery Worker 和 Celery Beat：

```bash
# 终端 1：后端
uv run main.py

# 终端 2：前端
npm --prefix web run dev

# 终端 3：Celery Worker
uv run celery --app app.shared.tasks.celery_app:celery_app worker -l INFO

# 终端 4：Celery Beat
uv run celery --app app.shared.tasks.celery_app:celery_app beat -l INFO
```

启动后访问：

- 前端：<http://localhost:7001>
- 后端 OpenAPI：<http://localhost:7000/docs>

修改 `docker/sandbox` 中的依赖或 Dockerfile 后，重新构建沙箱镜像：

```bash
docker compose -f docker/compose.yml build sandbox-image
```

## 启动后页面配置

使用 `conf/.env` 中配置的管理员账号登录前端，点击左下角的“后台”按钮进入“管理中心”。

### 1. 元数据导入与索引同步

1. 打开“元数据管理”，在“元数据 YAML 导入导出”区域选择 `conf/meta_config.yaml`。
2. 模式选择“全量替换”，点击“执行导入”。
3. 导入完成后，系统会自动提交字段和指标的语义索引同步任务。保持 Celery Worker 运行，等待任务完成后刷新页面，确认对应索引状态为“已同步”。
4. 在“表元数据”区域全选数据表，点击“全量同步取值索引”，完成启用取值索引字段的首次同步。

### 2. 数据库角色创建与权限分配

1. 打开“Doris 角色管理”，点击“添加角色”，填写角色标识、查询用户、业务描述和资源工作组后创建角色。
2. 选中创建的角色，在“表与列数据权限 (SELECT)”区域配置查询权限：表名留空表示授予当前数据库全部表权限；填写表名并将字段留空表示授予整表权限；同时填写表名和逗号分隔的字段表示仅授予指定字段权限。
3. 按需配置行级策略，并可将该角色设为新用户的默认角色。
4. 打开“用户账号管理”，添加或编辑用户，将 Doris 角色分配给需要查询数据的账号。
