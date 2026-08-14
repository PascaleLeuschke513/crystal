# Rihan Resource Hub

Rihan Resource Hub 是一个面向中文互联网内容创作者、网络资源整理者以及信息检索研究人员的综合性技术资源导航与外部链接管理系统。项目定位于为技术社区提供一个稳定、可扩展、具备分类管理能力的 URL 聚合与分发平台，解决个人或小型团队在维护大量外链资源时缺乏统一管理工具、链接失效追踪困难以及资源分类混乱等实际问题。通过结构化的数据组织方式和轻量级的静态生成逻辑，本项目能够高效处理数万级链接资源，并支持快速检索与分类展示。

本项目并非一个传统的内容提供网站，而是一个面向开发者与高级用户的资源索引基础设施。它专注于链接的采集、清洗、分类存储以及前端展示，并提供标准化的数据输出接口，便于第三方工具或脚本进行二次开发与数据迁移。项目内置了针对特定内容领域的链接分类策略，能够根据域名特征、路径结构以及来源标签进行自动归类和标记，极大降低了人工维护成本。同时，项目采用模块化设计，核心数据层与展示层完全解耦，允许用户自由替换前端模板或后端存储方案。

## 功能概览

- **多层级链接分类管理** 支持无限级分类目录，用户可根据主题、地区、语言或内容类型自定义分类树，每个链接可归属于多个分类标签，便于多维度检索。

- **批量链接导入与去重** 提供 CSV 和 JSON 格式的批量导入接口，自动检测并移除重复 URL 记录，同时保留原始导入时间与来源批次信息，方便数据溯源。

- **链接可用性健康检查** 内置异步链接探测模块，可定时检测所有存储 URL 的 HTTP 状态码、响应时间以及重定向链，生成健康度报告并标记异常链接。

- **全文检索与高级筛选** 基于倒排索引实现链接标题、描述、分类标签及来源站点的全文搜索，支持多条件组合筛选（状态码、分类、创建时间范围等）。

- **数据快照与回滚机制** 每次数据更新操作自动生成版本快照，用户可通过管理后台回溯至任意历史版本，防止误操作导致数据丢失。

- **RESTful API 输出接口** 所有链接数据均可通过 JSON API 方式对外提供，支持分页、排序、字段选择等标准查询参数，便于与第三方仪表盘或监控系统集成。

- **静态站点生成模式** 支持将链接数据一键导出为纯静态 HTML 文件，适合部署在 Nginx、Apache 或对象存储服务上，降低服务器资源消耗。

## 应用场景

- **个人知识库外链管理** 研究员或博主可使用本项目整理个人阅读清单、参考文献或工具推荐列表，通过分类和标签体系构建结构化知识网络，并定期运行健康检查以维护链接有效性。

- **社区资源聚合站点** 技术社区或兴趣小组可利用本项目搭建垂直领域的资源导航站，例如开源工具索引、学习资料汇总或行业资讯来源列表，通过 RESTful API 将数据同步至论坛或 CMS 系统。

- **运维监控与链接审计** 企业运维团队可将本项目作为内部链接资产管理工具，统一记录所有业务依赖的外部 API 文档、SDK 下载地址及第三方服务控制台入口，结合健康检查功能实现自动化可用性告警。

## 快速开始

以下步骤适用于 Linux/macOS 以及 Windows WSL 环境，确保系统已安装 Git 和 Node.js 18.x 及以上版本。

```bash
# 克隆项目仓库
git clone https://github.com/rihan-resource/rihan-hub.git
cd rihan-hub

# 安装项目依赖
npm install --production=false

# 复制环境变量模板并修改数据库连接配置
cp .env.example .env

# 执行数据库迁移与初始数据加载
npm run migrate
npm run seed

# 以开发模式启动本地服务，默认监听端口 3000
npm run dev
```

访问 http://localhost:3000 即可查看前端导航界面。如需构建生产环境静态文件，请执行 `npm run build` 并将 `dist/` 目录部署至 Web 服务器。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Node.js | 18.x LTS 或更高 | 运行时环境，建议使用 nvm 管理多版本 |
| npm | 9.x 或更高 | 包管理器，用于安装项目依赖 |
| SQLite | 3.35 或更高 | 默认嵌入式数据库，适合小型部署；生产环境可切换至 PostgreSQL |
| Git | 2.30 或更高 | 版本控制工具，用于克隆仓库和管理补丁 |
| Redis | 7.0 或更高 | 可选依赖，用于缓存 API 响应和会话存储（生产环境推荐） |
| Nginx | 1.20 或更高 | 可选依赖，用于反向代理和静态文件缓存（生产环境推荐） |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户手册 | /docs/user-guide/ | 如何添加链接、创建分类、运行健康检查以及使用搜索功能 |
| 开发指南 | /docs/developer-guide/ | 如何扩展数据模型、自定义分类策略以及编写新的 API 端点 |
| 部署运维 | /docs/deployment/ | 如何配置 Nginx 反向代理、设置 SSL 证书以及调优数据库连接池 |
| 数据规范 | /docs/data-spec/ | 链接数据结构的字段定义、支持的导入导出格式以及版本快照机制说明 |

## 资源列表

以下为本项目外部资源索引中收录的全部原始链接数据，按内容主题进行分组展示。所有 URL 均保留用户提供的原始格式，未做任何协议补全、域名规范化或路径改写。

**影视与娱乐内容类别**

<code>https://rihanshipinmianfeizaixianguankanb.org.cn</code>

<code>https://mianfeigaoqingshipinzaixianguankanb.org.cn</code>

<code>https://renqixiliezhongwenzimuwb.org.cn</code>

<code>https://rihanmeinvzhongwenzimub.org.cn</code>

**自然与生活内容类别**

<code>https://qingqingcaoyuanzhongwenzimub.org.cn</code>

**直播与网红内容类别**

<code>https://wanghongzhibozaixianshipin.org.cn</code>

<code>https://wanghongfulizhibo.org.cn</code>

## 项目结构

```
rihan-hub/
├── src/                           # 核心源代码目录
│   ├── api/                       # RESTful API 路由与控制层
│   │   ├── v1/                    # API v1 版本实现（含链接增删改查端点）
│   │   └── middleware/            # 身份验证、日志记录与速率限制中间件
│   ├── core/                      # 核心业务逻辑模块
│   │   ├── link-manager.js        # 链接生命周期管理（创建、更新、删除、软删除）
│   │   ├── classifier.js          # 基于规则引擎的自动分类器
│   │   └── health-checker.js      # 链接可用性异步探测与状态持久化
│   ├── models/                    # 数据模型与 ORM 映射（支持 SQLite 与 PostgreSQL）
│   │   ├── link.model.js          # 链接实体（含 URL、标题、描述、分类外键）
│   │   ├── category.model.js      # 分类树节点（支持无限级嵌套）
│   │   └── snapshot.model.js      # 版本快照元数据（时间戳、操作人、变更摘要）
│   ├── services/                  # 外部服务集成层
│   │   ├── redis-cache.js         # Redis 缓存封装（用于 API 响应加速）
│   │   └── static-generator.js    # 静态 HTML 站点生成器（基于 Handlebars 模板）
│   └── utils/                     # 通用工具函数库
│       ├── url-normalizer.js      # URL 标准化（去除跟踪参数、统一小写域名）
│       ├── deduplicator.js        # 批量链接去重算法（基于 SimHash 与精确匹配）
│       └── logger.js              # 结构化日志输出（支持 JSON 格式与文件轮转）
├── config/                        # 环境配置文件目录（开发、测试、生产）
│   ├── default.yaml               # 默认配置（端口、数据库路径、日志级别）
│   └── custom/                    # 用户自定义配置覆盖（不提交至版本库）
├── migrations/                    # 数据库迁移脚本（按时间戳排序）
│   ├── 001-initial-schema.sql     # 初始表结构（链接、分类、快照）
│   └── 002-add-indexes.sql        # 性能优化索引（分类路径、创建时间、状态码）
├── seed/                          # 初始示例数据（用于快速演示与测试）
│   └── demo-links.json            # 包含 50 条预置链接的 JSON 数据集
├── tests/                         # 单元测试与集成测试套件（基于 Mocha 与 Chai）
│   ├── unit/                      # 核心函数与工具类单元测试
│   └── integration/               # API 端点和数据库交互集成测试
├── docs/                          # 完整项目文档（用户手册、开发指南、部署说明）
│   ├── user-guide/                # 面向最终用户的操作文档
│   └── developer-guide/           # 面向贡献者的架构设计与扩展文档
├── public/                        # 前端静态资源（CSS、JavaScript、图片）
│   ├── css/                       # 基于 Tailwind CSS 的响应式样式
│   └── js/                        # 前端交互逻辑（搜索、分类展开、链接状态标记）
├── .env.example                   # 环境变量模板（数据库连接串、Redis 地址、JWT 密钥）
├── .gitignore                     # Git 忽略规则（node_modules、日志文件、本地配置）
├── package.json                   # npm 项目清单（依赖列表、脚本命令）
├── README.md                      # 项目主说明文档（即本文档）
└── LICENSE                        # MIT 许可证全文
```

## 贡献指南

我们欢迎社区开发者以多种形式参与本项目贡献，包括但不限于代码提交、文档改进、问题报告以及功能建议。请遵循以下步骤确保贡献流程顺畅。

1. 阅读项目行为准则与贡献者公约，确保所有交互符合开放、友好、包容的社区原则。在提交 Issue 或 Pull Request 前，请先在 Issues 列表中搜索是否已有类似话题，避免重复讨论。

2. 从 GitHub 仓库 Fork 本项目至个人账户，并在本地创建功能分支（建议命名格式为 `feature/简要描述` 或 `fix/问题编号`）。所有代码变更需附带对应的单元测试用例，确保测试覆盖率达到现有水平。

3. 代码提交时请遵循 Conventional Commits 规范（如 `feat: 添加链接批量导出功能` 或 `fix: 修复分类树递归查询溢出问题`），并确保所有现有测试通过。提交前运行 `npm run lint` 和 `npm run test` 进行本地校验。

4. 推送分支至个人 Fork 仓库后，通过 GitHub 界面发起 Pull Request 至主仓库的 `main` 分支。PR 描述中需清晰说明变更目的、实现方案以及可能的破坏性影响，并关联相关 Issue 编号。

5. 项目维护者将在 3 个工作日内进行代码审查，可能会提出修改意见或补充问题。待审查通过且所有 CI 检查（包括测试、构建、安全扫描）均为绿色状态后，PR 将被合并至主分支。

## 常见问题

**问：项目支持 MySQL 作为数据库后端吗？**

目前官方仅内置 SQLite 和 PostgreSQL 适配器。MySQL 的支持计划在 v2.0 路线图中。如果需要使用 MySQL，可自行修改 `src/models/` 中的数据库连接驱动并调整部分 SQL 语法（如自增列定义和索引创建语句）。我们欢迎社区贡献 MySQL 适配器实现。

**问：链接健康检查会影响源站性能吗？**

健康检查模块默认采用间隔随机抖动和单线程队列处理方式，每个目标 URL 的探测间隔最低为 24 小时，且并发请求数限制为 5 个。同时，请求头中设置了 `User-Agent` 为 `Rihan-Hub-HealthChecker/1.0` 并携带 `Cache-Control: no-cache`，但不会发送任何敏感信息。对于明显的高负载源站，可在配置文件中将检查频率调整为每周一次或手动禁用特定域名的检查。

**问：如何将现有书签或收藏夹数据导入本项目？**

项目提供了 `import` 命令行工具，支持从浏览器导出的 HTML 书签文件（Netscape 格式）以及标准 CSV 文件（需包含 `url,title,description` 列）。执行 `npm run import -- --file=bookmarks.html --format=html` 即可开始导入。对于大批量数据（超过 5000 条），建议分批次导入并开启 `--dedup` 去重选项以避免内存溢出。

## 许可证

MIT License

Copyright (c) 2026 Rihan Resource Hub Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
