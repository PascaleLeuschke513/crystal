# LinkPilot 技术资源导航站

LinkPilot 是一个面向开发者、技术研究人员与开源项目维护者的外链资源聚合与导航工具。项目核心定位为“技术资源的索引中枢”，通过结构化分类、可检索的链接库与轻量级元数据管理，帮助用户在海量技术文档、数据服务、实时信息源之间建立高效访问路径。

目标用户包括：需要频繁查阅外部数据接口的运维工程师、依赖多源信息进行决策的分析人员、以及希望为自身项目快速集成外部参考资源的开源开发者。LinkPilot 不存储任何第三方数据，仅作为链接的整理与展示层，通过静态站点生成与缓存策略，在保证访问速度的同时，最大程度保留原始信息源的权威性与实时性。

## 功能概览

- **分级链接目录系统**：支持按领域、用途、更新频率对链接进行多级分类，便于快速定位特定类型的技术资源。

- **实时状态检测面板**：对已收录的链接进行周期性可达性检查，并在界面中标记响应状态与最近检查时间，帮助用户识别已失效或响应缓慢的资源。

- **标签化全文检索**：基于链接标题、描述、标签及分类路径构建轻量级检索索引，支持模糊匹配与多标签组合筛选。

- **自定义收藏夹与分组**：允许用户将常用链接加入个人收藏分组，并支持通过浏览器本地存储或服务端账户体系进行跨设备同步。

- **链接元数据自动补全**：在收录新链接时，自动尝试抓取目标页面的标题、描述与图标信息，减少手动录入成本。

- **访问统计与热度排序**：记录每个链接在站内的被点击次数，提供按热度、新增时间、字母顺序等多种排序视图。

- **开放数据导出接口**：支持将整个链接目录导出为 JSON、YAML 或 CSV 格式，方便其他工具或脚本进行二次处理。

- **暗色主题与阅读模式**：提供视觉主题切换功能，并为文档类链接自动生成简化的阅读视图，提升长文阅读体验。

## 应用场景

**技术调研与选型**：当团队需要评估新的数据库中间件或监控工具时，LinkPilot 的“中间件”与“性能分析”分类下收录了主流项目的官方文档、性能对比报告以及社区评测文章，调研人员可在同一界面内完成多份资料的横向对照。

**赛事数据看板辅助**：对于需要实时关注体育赛事比分的技术演示项目或数据可视化大屏，LinkPilot 可集中收录多个比分信息源，并通过状态检测面板快速确认各数据源的可访问性，避免演示现场出现空白或超时故障。

**运维监控聚合**：运维人员可将内部监控系统、云服务状态页、第三方 API 健康检查端等链接统一纳入 LinkPilot，利用标签筛选功能按“生产环境”“测试环境”“第三方依赖”等维度快速切换视图，在故障排查时节约跳转时间。

**开源文档协作**：开源项目维护者可将项目依赖的规范文档、接口定义、参考实现等外部链接整理为 LinkPilot 中的一个共享目录，并将该目录链接放入项目 README 或 Wiki 中，降低新贡献者的信息获取门槛。

**个人知识库增强**：技术博主或知识管理爱好者可使用 LinkPilot 整理日常阅读的技术博客、官方公告、在线工具等资源，并利用收藏夹与标签功能构建个人化的技术信息网络。

## 快速开始

以下命令序列适用于 Linux / macOS / Windows WSL 环境，要求已安装 Git 与 Node.js 18.x 及以上版本。

```bash
# 克隆项目仓库
git clone https://github.com/linkpilot/linkpilot-core.git
cd linkpilot-core

# 安装项目依赖
npm install

# 复制环境配置模板并填充本地参数
cp .env.example .env

# 使用开发模式启动本地服务
npm run dev
```

执行完成后，访问控制台输出的本地地址（默认为 http://localhost:5173）即可进入 LinkPilot 实例，此时系统将加载预设的示例链接数据。若需使用完整功能，请参考后续文档导航章节配置数据库连接与缓存服务。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
| :--- | :--- | :--- |
| Node.js | 18.x 或 20.x LTS | 运行时环境，用于执行核心服务与构建脚本 |
| npm | 9.x 或 10.x | 包管理器，用于安装项目依赖与执行脚本命令 |
| PostgreSQL | 14.x 或 15.x | 主数据库，存储链接元数据、用户信息与访问统计 |
| Redis | 7.x | 缓存服务，用于状态检测结果缓存与会话管理 |
| Git | 2.30+ | 版本控制工具，用于克隆仓库与提交贡献 |
| PM2 | 5.x | 生产环境进程管理（可选，用于守护运行） |
| Nginx | 1.22+ | 反向代理与静态资源服务（生产部署推荐） |

## 文档导航

| 层面 | 目录位置 | 回答的问题 |
| :--- | :--- | :--- |
| 用户手册 | /docs/user-guide/ | 如何添加链接、创建分类、使用检索与收藏功能 |
| 管理员指南 | /docs/admin-guide/ | 如何配置状态检测间隔、管理用户权限、执行数据备份 |
| 开发文档 | /docs/developer-guide/ | 项目架构说明、API 接口规范、本地调试与打包流程 |
| 部署手册 | /docs/deployment/ | 支持 Docker Compose、Kubernetes 及传统虚拟机部署的方案说明 |
| 设计文档 | /docs/design/ | 数据库 ER 图、缓存策略、前端状态管理设计的详细说明 |
| 常见运维操作 | /docs/ops/ | 日志清理、数据迁移、SSL 证书续期的具体操作步骤 |

## 资源列表

本节列出本导航项目重点收录的外部技术资源，按内容领域分组展示。所有链接均严格保持原始格式输出。

赛事与比分信息

- <code>https://fajiajishibifenc.org.cn</code>
- <code>https://zuqiubisaijieguoc.org.cn</code>
- <code>https://yingchaobifenc.org.cn</code>
- <code>https://xijiabifenc.org.cn</code>
- <code>https://dejiabifenc.org.cn</code>
- <code>https://yijiabifenc.org.cn</code>
- <code>https://fajiabifenc.org.cn</code>

上述资源均为独立运营的第三方信息站点，LinkPilot 仅提供导航入口，不代理、不修改、不缓存其页面内容。用户访问时将直接跳转至原始域名，相关数据准确性及服务可用性由各站点自行负责。

## 项目结构

项目采用前后端分离的 Monorepo 组织方式，核心目录及注释如下：

```
linkpilot-core/
├── apps/
│   ├── web/                       # 前端 Vite + React 应用
│   │   ├── src/
│   │   │   ├── pages/             # 路由页面组件（首页、分类、收藏、统计）
│   │   │   ├── components/        # 可复用 UI 组件（链接卡片、搜索栏、状态标签）
│   │   │   ├── hooks/             # 自定义 React Hooks（请求、本地存储、主题）
│   │   │   ├── stores/            # Zustand 状态管理（用户、目录、筛选条件）
│   │   │   └── utils/             # 前端工具函数（URL 格式化、时间转换）
│   │   ├── public/                # 静态资源（favicon、默认图标）
│   │   └── vite.config.ts         # Vite 构建配置（代理、别名、压缩）
│   └── server/                    # 后端 NestJS 应用
│       ├── src/
│       │   ├── modules/
│       │   │   ├── links/         # 链接管理模块（增删改查、元数据抓取）
│       │   │   ├── checks/        # 状态检测模块（定时任务、结果存储）
│       │   │   ├── users/         # 用户与认证模块（JWT、本地策略）
│       │   │   └── stats/         # 统计模块（点击记录、热度计算）
│       │   ├── config/            # 配置中心（环境变量、数据库连接）
│       │   └── migrations/        # 数据库迁移脚本（TypeORM）
│       └── test/                  # 单元测试与 e2e 测试用例
├── packages/
│   ├── shared-types/              # 前后端共享 TypeScript 类型定义
│   └── lint-config/               # ESLint + Prettier 统一配置
├── docker/
│   ├── Dockerfile.web             # 前端生产镜像构建文件
│   ├── Dockerfile.server          # 后端生产镜像构建文件
│   └── docker-compose.yml         # 本地开发与生产部署编排文件
├── docs/                          # 完整文档体系（见上文文档导航）
├── scripts/
│   ├── seed-demo-data.js          # 初始化示例数据脚本
│   └── health-check.sh            # 外部依赖可达性检测脚本
├── .env.example                   # 环境变量配置模板
├── .gitignore                     # Git 忽略规则
├── package.json                   # 根项目依赖与工作区配置
├── pnpm-workspace.yaml            # pnpm Monorepo 工作区定义
└── README.md                      # 项目入口说明文档（即本文档）
```

## 贡献指南

我们欢迎并感谢任何形式的贡献，包括但不限于新增链接分类、改进界面样式、修复文档错误以及提交功能建议。请遵循以下步骤参与贡献：

1. 在 GitHub 上 Fork 本仓库，并将 Fork 后的仓库克隆至本地开发环境。建议在 dev 分支上进行所有修改，保持 main 分支与上游同步。

2. 创建新的功能分支，分支命名采用 `feat/`、`fix/`、`docs/` 前缀，例如 `feat/add-dark-theme`。提交信息请遵循 Conventional Commits 规范，确保每次提交具有明确语义。

3. 若涉及新增外部链接资源，请在 `/apps/server/src/modules/links/link-seed.json` 中按格式添加条目，并确保目标链接不包含恶意内容或侵犯第三方权益。对于已有链接的更新或删除，请在相关 Issue 中说明理由。

4. 本地通过所有单元测试与 ESLint 检查后，推送分支至您的远程仓库，并在 GitHub 上向本仓库的 dev 分支发起 Pull Request。PR 描述中请清晰列出改动点、测试覆盖情况以及是否影响现有功能。

5. 项目维护者将在 3 个工作日内进行 Review，可能会要求补充测试用例或调整实现方式。合并后您的贡献将被列入项目贡献者列表（如有要求）。

## 常见问题

**Q：LinkPilot 是否存储或缓存第三方链接的具体内容？**

A：不存储。LinkPilot 仅保存链接的标题、描述、分类和 URL 地址，以及用户主动记录的标签与收藏状态。状态检测功能仅发送 HTTP HEAD 请求验证可达性，不抓取页面正文内容。所有跳转均通过前端原生 window.location 或 a 标签直接访问原始地址，不经过任何代理服务器。

**Q：如何迁移 LinkPilot 的数据到另一个实例？**

A：数据迁移主要通过 PostgreSQL 的 pg_dump / pg_restore 工具完成。停止目标实例的服务，执行完整数据库导出，再导入新环境的同名数据库中。同时需要同步 Redis 缓存中的会话数据（如使用持久化模式）以及 `.env` 配置中的加密密钥。对于仅迁移链接数据而不包含用户信息，可以使用项目提供的 `scripts/export-links.js` 导出为 JSON 后在新实例中通过管理后台导入。

**Q：状态检测显示某个链接不可达，但浏览器中可以直接访问，为什么？**

A：这通常是由于目标服务器对 HEAD 请求的限制或防火墙策略导致。LinkPilot 默认使用 HEAD 方法进行快速检测，某些站点会拒绝 HEAD 请求但允许 GET 请求。您可以在管理后台将该链接的检测模式调整为“GET 模式”，或手动设置检测超时时间为 5000 毫秒以上。若问题持续，可在该链接的备注中标记“忽略检测”状态，系统将不再对其告警。

## 许可证

MIT License

Copyright (c) 2026 LinkPilot Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
