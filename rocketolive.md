# Resource Navigator

Resource Navigator is a comprehensive technical resource aggregation and navigation system designed for developers, researchers, and technical content curators who need to organize, categorize, and rapidly access distributed online materials. The project addresses the critical challenge of managing disparate external resource links across multiple domains by providing a structured, maintainable, and queryable index layer that sits on top of existing content providers.

Targeting system administrators, DevOps engineers, and technical documentation maintainers, Resource Navigator offers a lightweight yet extensible framework for building curated resource catalogs that can be deployed as standalone static sites or integrated into existing documentation pipelines. It solves the problem of link rot, organizational fragmentation, and discovery friction by centralizing resource metadata, enforcing consistent annotation schemas, and providing multiple access pathways including full-text search, category filtering, and chronological sorting.

## 功能概览

**声明式资源索引** - 支持通过 YAML 和 JSON 格式定义资源条目，包括标题、描述、标签、优先级、状态检查和更新频率等元数据字段，便于自动化处理。

**多维度分类体系** - 内置标签系统和层级化分类器，允许同一资源归属于多个逻辑分组，支持自定义分类维度如内容类型、语言、地域和主题领域。

**健康状态监控** - 定期执行 HTTP 头检查、SSL 证书验证和响应时间测量，自动标记不可用或响应缓慢的资源，生成可用性趋势报表。

**智能搜索与过滤** - 基于全文检索引擎实现资源标题、描述和标签的模糊匹配，支持布尔表达式、字段限定和范围查询，返回按相关度排序的结果。

**快照与归档机制** - 对关键资源内容进行定时快照存储，生成内容指纹用于变更检测，提供历史版本回溯能力，降低外部内容变更或删除带来的影响。

**访问统计与热度分析** - 记录资源点击、搜索和引用事件，生成访问热力图和趋势曲线，辅助识别高频使用资源和潜在淘汰资源。

**数据导入导出管道** - 提供 CSV、JSON、XML 和 RSS 格式的批量导入导出接口，支持从现有书签文件、浏览器收藏夹或数据库迁移数据。

**权限与协作模型** - 基于角色的访问控制区分管理员、编辑者和只读用户，支持资源审核流程、变更日志审计和注释讨论线程。

## 应用场景

**技术文档门户维护** - 企业技术文档团队使用 Resource Navigator 管理分散在各内部系统和外部平台的 API 文档、SDK 下载链接和示例代码仓库，通过健康监控自动发现失效链接并在文档中生成警告标记。

**开源项目资源站** - 开源社区维护者构建项目相关的学习资源导航页，聚合教程视频、配套代码仓库、讨论区和第三方工具列表，利用分类体系让新手和高级开发者都能快速定位所需材料。

**学术研究资料管理** - 研究团队整理领域内相关数据集、论文预印本、代码实现和线上工具，通过快照机制保存关键参考资料的副本，避免研究过程中外部链接变动影响实验可复现性。

**运维知识库构建** - 运维团队将内部监控面板、日志系统、部署文档和故障处理手册的外部链接纳入统一导航，配合访问统计识别常用运维入口，优化应急响应流程。

**内容聚合与推荐系统** - 内容运营团队创建主题资源合集，利用多维度分类和搜索过滤能力生成动态推荐列表，通过 RSS 导出将资源更新同步到订阅者终端。

## 快速开始

```bash
# 克隆项目仓库
git clone https://github.com/resourcenavigator/core.git resource-navigator
cd resource-navigator

# 安装依赖（使用 pnpm 或 npm）
pnpm install

# 复制环境配置模板并填写必要参数
cp .env.example .env

# 初始化数据库结构并加载默认分类数据
pnpm run db:init
pnpm run seed:categories

# 启动开发服务器，默认监听 3000 端口
pnpm run dev

# 生产环境构建静态输出
pnpm run build
pnpm run start
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Node.js | >= 20.0.0 | 运行时环境，推荐使用 LTS 版本 |
| pnpm | >= 8.0.0 | 包管理器，也可使用 npm 9+ 或 yarn 4+ |
| PostgreSQL | >= 15.0 | 主数据库，用于存储资源元数据、分类和访问记录 |
| Redis | >= 7.0 | 缓存层，用于搜索索引和会话管理，可配置为可选降级模式 |
| Elasticsearch | >= 8.10 | 全文检索引擎，用于高级搜索功能，可替换为 Meilisearch |
| MinIO / S3 | >= 2024 | 对象存储，用于快照文件和归档数据保存 |
| Docker | >= 24.0 | 容器化部署建议，开发环境可使用 Docker Compose 一键启动依赖服务 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 入门指南 | /docs/getting-started/ | 如何从零开始部署 Resource Navigator，包括环境准备、首次启动和基础配置 |
| 架构设计 | /docs/architecture/ | 系统各模块的职责划分、数据流转路径、扩展点设计和性能考量 |
| API 参考 | /docs/api/ | 所有 RESTful 接口的请求格式、响应结构、错误码和调用示例 |
| 插件开发 | /docs/plugins/ | 如何编写自定义分类器、导入导出格式扩展和健康检查策略插件 |

## 资源列表

内容分类索引

视频直播类资源
- <code>https://renqixiliezhongwenzimuwb.org.cn</code>
- <code>https://rihanmeinvzhongwenzimub.org.cn</code>
- <code>https://qingqingcaoyuanzhongwenzimub.org.cn</code>
- <code>https://wanghongzhibozaixianshipin.org.cn</code>
- <code>https://wanghongfulizhibo.org.cn</code>
- <code>https://guochanwanghongzhibozhuzaixian.org.cn</code>
- <code>https://guochanwanghongshipinzhibo.org.cn</code>

## 项目结构

```
resource-navigator/
├── apps/
│   ├── web/                         # 主 Web 应用（Next.js 14，App Router）
│   ├── api/                         # RESTful API 服务（Express.js + TypeScript）
│   └── worker/                      # 后台任务处理器（健康检查、快照、统计聚合）
├── packages/
│   ├── core/                        # 核心数据模型、验证器、类型定义
│   ├── crawler/                     # 资源内容抓取与快照生成模块
│   ├── search/                      # 搜索引擎适配层（Elasticsearch / Meilisearch）
│   ├── storage/                     # 对象存储抽象接口（本地文件 / S3 / MinIO）
│   └── ui/                          # 共享 UI 组件库（React + Tailwind CSS）
├── configs/
│   ├── eslint/                      # 统一 ESLint 配置（Flat Config）
│   ├── tsconfig/                    # TypeScript 编译选项（继承链）
│   └── vitest/                      # 单元测试预设配置
├── deployments/
│   ├── docker/                      # Dockerfile 和容器编排脚本
│   ├── kubernetes/                  # K8s 部署清单（Deployment, Service, Ingress）
│   └── terraform/                   # 云基础设施即代码（AWS / 阿里云）
├── scripts/
│   ├── migrate/                     # 数据库迁移脚本（Knex.js）
│   ├── seed/                        # 初始分类和示例数据填充
│   └── benchmark/                   # 性能压测脚本（k6）
├── docs/                            # 完整文档源文件（Markdown + Mermaid）
├── tests/
│   ├── unit/                        # 单元测试（Vitest）
│   ├── integration/                 # 集成测试（Supertest + Testcontainers）
│   └── e2e/                         # 端到端测试（Playwright）
├── .env.example                     # 环境变量配置模板
├── docker-compose.yml               # 本地开发依赖服务编排
├── package.json                     # 根项目依赖和 Workspace 配置
├── turbo.json                       # Turborepo 流水线任务定义
└── README.md                        # 项目说明文档（本文件）
```

## 贡献指南

1. 查阅问题追踪器中的 `good-first-issue` 标签列表，选择适合初学者的任务，或在讨论区提出新的功能建议并获得核心维护者反馈后再开始实现。

2. 派生项目仓库到个人账户，创建以功能名称或问题编号命名的主题分支，遵循 Conventional Commits 规范编写提交信息，确保每次提交保持逻辑原子性。

3. 在本地环境运行 `pnpm run test` 确保所有测试通过，并为新增功能或修复编写相应的单元测试和集成测试用例，测试覆盖率不低于百分之八十。

4. 提交拉取请求前运行 `pnpm run lint` 和 `pnpm run format` 统一代码风格，填写 PR 模板中的所有检查项，清晰描述变更动机、实现方式和影响范围。

5. 等待至少两名核心维护者进行代码审查，根据反馈意见修改代码，审查通过后将由维护者执行合并操作并触发自动部署流程。

## 常见问题

**问：Resource Navigator 是否支持完全离线部署，不依赖任何外部云服务？**

答：支持。系统设计了可替换的后端适配器，Elasticsearch 可切换为内嵌的 SQLite FTS5 全文搜索，Redis 缓存可以降级为内存存储，对象存储可使用本地文件系统。通过配置 `DEPLOYMENT_MODE=offline` 即可启用完全离线模式，所有数据保存在本地磁盘。

**问：如何处理外部资源链接变更或内容被删除的情况？**

答：系统提供三层应对机制。第一层是定时健康检查，每小时探测资源可访问性并记录状态变更。第二层是内容快照，对标记为高优先级的资源进行每日抓取并存储内容指纹，当检测到内容差异时发送告警。第三层是手动覆写，编辑者可以为资源条目添加重定向映射或替代链接，原有链接将自动显示废弃标记并引导用户访问新地址。

**问：能否将 Resource Navigator 作为 npm 包引入到现有项目中而不是独立部署？**

答：可以。核心数据模型和搜索适配器已打包为 `@resourcenavigator/core` 和 `@resourcenavigator/search` 两个独立的 npm 包，支持通过 `import { ResourceIndex, SearchEngine } from '@resourcenavigator/core'` 的方式嵌入到任意 Node.js 应用中。Web 界面组件库 `@resourcenavigator/ui` 也可单独安装并集成到 React 项目中使用。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:08:31
