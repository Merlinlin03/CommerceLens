# CommerceLens

> **电商智能问数与归因分析平台** —— 让业务人员用自然语言直接问数据、自动定位波动根因。

[![Python](https://img.shields.io/badge/Python-3.11+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![Vue](https://img.shields.io/badge/Vue-3-4FC08D?logo=vue.js&logoColor=white)](https://vuejs.org/)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white)](https://docs.docker.com/compose/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](#license)

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
│ 前端 (Vue 3 + Vite) │
│ 对话问数 · 归因报告 · 管理中心 │
└────────────────────────────┬────────────────────────────────┘
│ HTTP / SSE
┌────────────────────────────▼────────────────────────────────┐
│ 后端 (FastAPI + Celery) │
│ ┌──────────┬──────────┬──────────┬──────────┬──────────┐ │
│ │ Identity │ Metadata │ Query │Assistant │Workflows │ │
│ │ 认证授权 │ 语义召回 │ 安全查询 │ 多Agent │ 跨存储 │ │
│ └──────────┴──────────┴──────────┴──────────┴──────────┘ │
│ ┌──────────────────────────────────────────────────────┐ │
│ │ Shared 基础能力层 │ │
│ └──────────────────────────────────────────────────────┘ │
└───────┬──────────────┬──────────────┬──────────────┬────────┘
│ │ │ │
┌────▼────┐ ┌────▼────┐ ┌────▼────┐ ┌────▼────┐
│ Doris │ │PostgreSQL│ │ Redis │ │ ES │
│ 数据仓库│ │ 元数据/ │ │ 缓存/ │ │ 语义 │
│ │ │ 认证/LG │ │ 队列 │ │ 索引 │
└─────────┘ └─────────┘ └─────────┘ └─────────┘
│
┌────────▼────────┐
│ Sandbox 沙箱 │
│ (Docker 隔离) │
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

