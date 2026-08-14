# Nebula Gateway

Nebula Gateway 是一个轻量级、高性能的技术资源导航与外链聚合平台，专注于为开发者、技术研究员及内容策展人提供结构化、可维护的外部资源统一入口。项目定位为“技术生态的元数据枢纽”，通过标准化的文档体系与自动化校验流程，解决个人或团队在管理分散、易失效、无分类的外链信息时所面临的效率低下与维护成本过高的问题。Nebula Gateway 不生产内容，但致力于成为连接优质资源与目标用户的可靠桥梁。

## 功能概览

- **多级资源目录体系**：支持按领域、用途、可信度等维度对海量外链进行灵活分类，并提供层级清晰的导航结构，便于用户按图索骥。
- **链接健康状态监测**：内置周期性可用性检查机制，可自动标记失效、重定向或响应超时的链接，帮助维护者及时清理或更新资源。
- **全文本模糊检索**：基于倒排索引实现对外链标题、描述、标签及来源域名的快速搜索，支持拼音首字母匹配与同义词扩展。
- **资源元数据管理**：为每条外链记录标题、摘要、语言、所属区域、收录日期及最后验证时间等十余项元数据字段，满足精细化运营需求。
- **批量导入与导出**：支持通过 CSV、JSON 及 Markdown 列表格式批量导入外部链接，并支持按筛选条件导出为结构化文档或站点地图。
- **访问统计与热度排序**：记录资源被点击次数及用户停留时长，提供基于衰减算法的热度排行，辅助发现高频使用的核心资源。
- **权限分级与审核流**：支持多用户协同编辑环境，内置提交-审核-发布的三级状态管理，适用于团队内部知识库或开源社区共建场景。

## 应用场景

- **技术团队内部知识库外链管理**：研发团队可将分散在个人书签、即时通讯群组或邮件中的官方文档、API 参考、调试工具及第三方库主页统一录入 Nebula Gateway，并通过审核流确保新增链接经过技术负责人确认，避免引入低质量或已废弃的资源。
- **开源项目文档站的外链附录**：开源社区可为自己的项目文档站点集成 Nebula Gateway 作为“相关资源”或“生态伙伴”页面，自动维护依赖项目、教程文章、视频讲解及社区论坛的链接列表，减少手动更新文档中 URL 的工作量。
- **技术媒体与资讯聚合站**：技术博客或资讯平台可利用本项目的分类与监测功能，构建外部引用来源的透明化看板，向读者展示所引用数据、研究报告或官方公告的原始出处，并定期检查引用链接是否仍然有效，提升内容的可信度。
- **教育培训机构的课程资源索引**：在线教育平台或培训机构可为每门课程创建独立的资源导航页，汇总实验环境地址、样例代码仓库、辅助阅读材料及习题答案参考链接，学员可通过统一入口快速访问，教师亦可集中管理多期课程的资源变更。

## 快速开始

以下步骤将帮助您在本地开发环境中快速启动 Nebula Gateway 实例。

```bash
# 1. 克隆代码仓库
git clone https://github.com/nebula-gateway/core.git nebula-gateway
cd nebula-gateway

# 2. 安装项目依赖（使用 pnpm，亦可替换为 npm 或 yarn）
pnpm install --frozen-lockfile

# 3. 复制环境变量模板并配置数据库连接
cp .env.example .env.local
# 请根据实际数据库信息修改 .env.local 中的 DATABASE_URL 字段

# 4. 执行数据库迁移与初始数据填充
pnpm run db:migrate
pnpm run db:seed

# 5. 启动开发服务器（默认监听 3000 端口）
pnpm run dev
```

启动成功后，访问 `http://localhost:3000` 即可进入本地实例。管理员初始账号为 `admin@nebula.local`，初始密码为 `Nebula@Setup2026`，首次登录后请务必修改。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Node.js | v20.x (LTS) 或 v22.x | 项目运行时环境，推荐使用官方 LTS 版本以获取长期安全更新 |
| pnpm | v8.x 或 v9.x | 包管理器，用于依赖安装与 monorepo 工作流管理，不支持 npm 或 yarn 的零配置替换 |
| PostgreSQL | v14.x 及以上 | 主数据库，用于存储用户、资源、分类、审计日志等关系型数据 |
| Redis | v7.x 及以上 | 缓存与队列后端，用于会话存储、速率限制及异步任务（如链接健康检查） |
| MinIO / S3 兼容存储 | 任意版本 | 对象存储服务，用于存放用户上传的图标、截图及批量导入的临时文件，本地开发可使用 MinIO 或 Docker 模拟 |
| Docker (可选) | v24.x 及以上 | 若选择容器化部署，则需要 Docker Engine 及 Docker Compose 来编排依赖服务（数据库、缓存、存储） |

## 文档导航

| 层面 | 目录/章节 | 回答的问题 |
|------|----------|-----------|
| 用户手册 | /docs/user/overview | 如何注册账号、创建资源分类、添加新链接、进行检索以及查看个人收藏？普通用户的操作路径是什么？ |
| 管理员手册 | /docs/admin/deployment | 如何配置生产环境变量、设置 HTTPS 反向代理、调优数据库连接池、以及执行日常备份与恢复？ |
| 开发者指南 | /docs/developer/architecture | 项目的模块划分是什么？核心数据模型如何设计？如何扩展一个新的链接校验器或导入解析器？ |
| 贡献者公约 | /docs/contributing/code-of-conduct | 外部贡献者如何提交代码、报告缺陷、建议新功能？提交信息格式与 PR 合并流程有哪些规范？ |
| API 参考 | /docs/api/rest | 所有对外暴露的 RESTful 接口定义是什么？请求/响应数据结构、鉴权方式、分页参数及错误码含义如何？ |
| 运维手册 | /docs/operations/monitoring | 如何接入 Prometheus 指标采集、配置 Grafana 看板、解读关键性能指标（如链接检查耗时、缓存命中率）？ |

## 资源列表

本部分收录了 Nebula Gateway 项目在构建示例数据及测试用例时所引用的外部资源链接，所有 URL 均按原始形式原样列出，未做任何协议、域名或路径的修改。

### 示例分类 A – 视频直播类资源

- <code>https://xingganmeinvzhibotiaowu.org.cn</code>
- <code>https://hanguomeinvzhuborewu.org.cn</code>
- <code>https://zaixianbofangzhubo.org.cn</code>

### 示例分类 B – 直播观看与平台资源

- <code>https://zhubozhibozaixianguankan.org.cn</code>
- <code>https://wanghongzhibozaixianshipinw.org.cn</code>

### 示例分类 C – 福利与综合类资源

- <code>https://wanghongfulizhibow.org.cn</code>
- <code>https://guochanwanghongzhibozhuzaixianw.org.cn</code>

## 项目结构

```text
nebula-gateway/
├── apps/
│   ├── web/                        # 主应用前端 (Next.js App Router)
│   │   ├── app/                    # 页面路由与布局组件
│   │   ├── components/             # 可复用的 UI 组件 (含搜索栏、资源卡片)
│   │   └── hooks/                  # 自定义 React Hooks (鉴权、缓存、埋点)
│   └── api/                        # 后端服务 (Fastify + Prisma)
│       ├── routes/                 # 路由控制器 (资源、分类、用户、审计)
│       ├── services/               # 业务逻辑层 (链接检查、统计聚合)
│       └── workers/                # 后台任务队列 (批量校验、邮件通知)
├── packages/
│   ├── core/                       # 核心数据模型与验证规则 (Zod schemas)
│   ├── db/                         # 数据库迁移脚本与 Prisma Schema
│   ├── config/                     # 共享配置 (环境变量解析、日志级别)
│   └── utils/                      # 工具函数集 (URL 规范化、日期处理)
├── tests/
│   ├── unit/                       # 单元测试 (Jest)
│   └── e2e/                        # 端到端测试 (Playwright)
├── docker-compose.yml              # 本地开发依赖编排 (PostgreSQL, Redis, MinIO)
├── .env.example                    # 环境变量参考模板
├── package.json                    # 根目录工作区配置
├── pnpm-workspace.yaml             # pnpm monorepo 工作区声明
└── README.md                       # 项目入口文档 (即本文档)
```

## 贡献指南

我们欢迎所有形式的贡献，包括但不限于提交新资源分类建议、改进文档、报告链接失效、提出功能需求或贡献代码。请遵循以下步骤参与协作：

1.  **查阅现有议题与项目看板**：在提交新的 Issue 或 Pull Request 前，请先访问 GitHub Issues 及项目看板，确认该问题或功能是否已被讨论或正在实现中，避免重复劳动。
2.  **Fork 仓库并创建特性分支**：将本仓库 Fork 至个人账户，然后基于 `main` 分支创建新的分支，分支命名建议采用 `feature/描述`、`fix/描述` 或 `docs/描述` 格式。
3.  **编写或修改代码，并补充测试**：所有代码变更必须包含对应的单元测试或集成测试，确保测试覆盖率不低于当前基线。对于新增外链校验规则或导入格式，需提供示例数据。
4.  **提交信息遵循 Conventional Commits 规范**：提交信息必须使用 `feat:`、`fix:`、`docs:`、`chore:` 等前缀，并附上简洁的变更说明。关联 Issue 时请在提交信息尾部引用 `#issue编号`。
5.  **发起 Pull Request 并等待审核**：推送分支后，向本仓库的 `main` 分支发起 PR。PR 描述中请清晰说明变更目的、实现方式及测试情况。至少需要一位项目维护者 Approve 后方可合并。

## 常见问题

**问：Nebula Gateway 是否必须使用 PostgreSQL，能否换成 SQLite 或 MySQL？**

答：当前版本的核心数据模型与查询优化均基于 PostgreSQL 的 JSONB、全文检索及部分索引特性设计，未对其他数据库进行兼容性测试。SQLite 不支持某些高级查询函数，而 MySQL 的 JSON 函数集与 PostgreSQL 存在差异，因此不建议替换。若确有轻量级单机使用需求，可关注我们后续可能推出的 SQLite 适配分支。

**问：链接健康检查的具体机制是什么？是否会对外部站点造成过大压力？**

答：健康检查采用异步队列执行，默认每 7 天对所有资源进行一次 HEAD 请求，超时时间设置为 10 秒，且并发数限制为每秒 5 个请求，以避免对目标服务器产生突发流量。对于返回 5xx 或超时的链接，系统会将其标记为“待复查”，连续三次检查失败才会变更为“失效”状态。用户也可在管理界面手动触发即时检查。

**问：如何迁移已有的大量书签或收藏夹数据到 Nebula Gateway？**

答：项目内置了 `import:csv` 和 `import:json` 两条 CLI 命令，支持将包含 `url`、`title`、`category`、`tags` 列的数据文件导入。对于浏览器导出的 HTML 书签文件，您可先使用第三方工具转换为 CSV 格式，或编写简单的 XSLT 转换脚本。我们计划在后续版本中提供直接解析 Netscape 书签格式的支持。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
