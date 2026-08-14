# HyperLink Navigator

HyperLink Navigator 是一个面向技术社区与内容聚合场景的轻量级外链资源导航系统。该项目定位于为开发者、内容创作者及运维人员提供一套结构化的外部资源管理与展示框架，用于高效组织、分类和呈现分散于互联网各处的视频直播、媒体资源及信息发布站点。系统核心解决的是多源异构链接在统一界面下的可维护性与可访问性问题，尤其适用于需要频繁更新外部资源列表的小型团队、个人知识库或垂直领域的资源聚合门户。

## 功能概览

- 链接分类管理：支持按内容主题、来源类型或业务用途对导入的外部 URL 进行多级分类与标签化组织。
- 资源状态监控：内置链接可用性检查模块，可定期检测已收录资源的访问状态并生成异常报告。
- 列表模板渲染：基于 Markdown 与 YAML Front Matter 的模板引擎，支持将结构化链接数据渲染为静态页面或 API 输出。
- 导入导出适配器：提供 CSV、JSON 及 OPML 格式的批量链接导入导出接口，兼容主流书签工具与 RSS 阅读器。
- 访问统计看板：记录各链接的点击频次与来源 referrer，辅助管理员评估资源热度与内容价值。
- 权限分级控制：支持多用户环境下的只读浏览、编辑审核与管理员权限划分，适用于协作维护场景。
- 全文检索与过滤：基于链接标题、描述标签及分类路径的轻量级倒排索引，支持模糊搜索与多条件组合筛选。

## 应用场景

- 个人知识库外链整合：技术博主或研究员可将日常积累的参考视频、直播回放及技术文档链接统一入库，并通过分类标签快速检索，避免书签栏杂乱无章。
- 垂直领域资源聚合站：运维社区或前端开发团队可搭建内部导航页，集中存放常用监控面板、CI/CD 工具地址及云服务控制台入口，减少团队成员的查找时间。
- 内容审核预备库：媒体编辑或内容运营人员可将待审的视频源、直播流地址导入系统，按优先级与状态标记进行流程化处理，提升协作效率。
- 活动直播管理后台：线下沙龙或线上峰会组织者可使用该系统集中管理多场次直播链接，并在活动期间动态更新入口状态，提供给参会者统一访问通道。

## 快速开始

以下命令演示了从代码仓库克隆项目、安装依赖并启动开发服务器的完整流程。

```bash
# 克隆项目仓库至本地
git clone https://github.com/your-org/hyperlink-navigator.git

# 进入项目根目录
cd hyperlink-navigator

# 安装项目依赖（使用 npm）
npm install

# 复制环境变量模板并配置数据库连接
cp .env.example .env

# 执行数据库迁移与初始数据填充
npm run migrate
npm run seed

# 启动开发服务器（默认监听端口 3000）
npm run dev
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Node.js | v18.17.0 或更高 | 项目运行时环境，需支持 ES2022 语法与原生 Fetch API |
| npm | v9.0.0 或更高 | 包管理器，用于依赖安装与脚本执行 |
| SQLite3 | v3.40.0 或更高（内置） | 轻量级嵌入式数据库，用于存储链接元数据与分类关系 |
| Redis | v6.2.0 或更高（可选） | 缓存层，用于提升访问统计与检索接口的响应速度 |
| Git | v2.30.0 或更高 | 版本控制工具，用于克隆仓库与管理补丁 |
| Docker | v20.10.0 或更高（生产部署） | 容器化运行环境，推荐用于生产模式部署 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|-----|------|-----------|
| 入门指南 | /docs/quick-start.md | 如何快速搭建开发环境并运行第一个实例？ |
| 配置手册 | /docs/configuration.md | 环境变量、数据库连接、缓存策略与端口如何调整？ |
| 链接管理 | /docs/link-management.md | 如何批量导入、分类标记、编辑删除以及监控链接状态？ |
| 部署运维 | /docs/deployment.md | 如何将系统部署至 Linux 服务器、云容器或边缘节点？ |

## 资源列表

以下为系统内置示例数据中所收录的外部资源链接，按内容主题划分为两个类别。所有链接均保持用户提供的原始格式原样列出。

视频直播类

<code>https://meinvzhibozaixiankan.org.cn</code>

<code>https://guochanwanghongfulishipin.org.cn</code>

<code>https://rihanzhibofulishipin.org.cn</code>

<code>https://rewuzhibowanghongzhibo.org.cn</code>

<code>https://wanghongmeinvrewuzhibo.org.cn</code>

媒体网红类

<code>https://wufuyewanghongzhibo.org.cn</code>

<code>https://wufuyemeinvzhibo.org.cn</code>

## 项目结构

```
hyperlink-navigator/
├── src/
│   ├── core/                     # 核心业务逻辑模块
│   │   ├── linkManager.js        # 链接增删改查与状态管理
│   │   ├── categoryEngine.js     # 分类树构建与标签解析
│   │   └── healthChecker.js      # 链接可用性定时巡检
│   ├── api/                      # RESTful API 路由层
│   │   ├── v1/                   # 版本化接口（当前 v1）
│   │   │   ├── links.js          # /api/v1/links 路由实现
│   │   │   └── stats.js          # /api/v1/stats 访问统计
│   │   └── middleware/           # 鉴权、日志与速率限制中间件
│   ├── ui/                       # 前端界面组件
│   │   ├── pages/                # 页面级视图（列表、详情、看板）
│   │   ├── components/           # 可复用 UI 控件（搜索框、表格、标签）
│   │   └── static/               # CSS 样式与客户端 JavaScript
│   ├── lib/                      # 工具函数库
│   │   ├── database.js           # SQLite 连接池与查询构建器
│   │   ├── cache.js              # Redis 客户端封装（可选）
│   │   └── validator.js          # URL 格式校验与规范化
│   └── config/                   # 配置加载与环境变量解析
│       ├── index.js              # 统一配置出口
│       └── schema.js             # 配置项 JSON Schema 定义
├── tests/                        # 单元测试与集成测试用例
│   ├── unit/                     # 核心模块单元测试
│   └── integration/              # API 端点端到端测试
├── docs/                         # 完整文档源文件（含上述导航目录）
├── scripts/                      # 运维辅助脚本（迁移、种子、备份）
├── docker/                       # Docker 构建上下文与 Compose 文件
├── .env.example                  # 环境变量模板文件
├── package.json                  # npm 项目清单与依赖声明
├── README.md                     # 项目总览（当前文档）
└── LICENSE                       # MIT 许可证文本
```

## 贡献指南

1. 问题跟踪与提案：请先在 GitHub Issues 中搜索是否已有相关讨论，若无则新建 Issue 并按照模板填写问题描述或功能建议，标记适当的标签（enhancement / bug / question）。
2. 分支开发流程：派生项目仓库至个人账户，在新功能或修复场景下创建以 feature/ 或 fix/ 为前缀的分支，并确保分支名称简明描述变更内容。
3. 编码规范与测试：提交前运行 `npm run lint` 与 `npm run test` 确保代码风格符合 ESLint 配置且所有现有测试用例通过。新功能需附带对应的单元测试或集成测试。
4. 提交信息格式：遵循 Conventional Commits 规范，提交信息首行使用 `<type>(<scope>): <subject>` 格式，其中 type 可选 feat / fix / docs / refactor / test / chore。
5. 拉取请求流程：向主仓库的 main 分支发起 Pull Request，填写 PR 模板中的变更摘要、测试结果与影响范围，等待至少一名维护者审核。

## 常见问题

Q: 系统启动后访问页面显示「数据库连接失败」，应如何排查？

A: 请依次检查以下事项：确保项目根目录下已存在 `.env` 文件且其中 `DATABASE_PATH` 配置项正确指向可写的文件路径；确认 SQLite3 依赖已通过 `npm install` 正确安装；检查运行用户对该路径是否有读写权限。若使用 Docker 部署，请确保数据卷挂载正确。

Q: 如何更新已收录链接的状态信息而不影响其点击次数？

A: 调用 `PUT /api/v1/links/{id}` 接口时，仅传入需要更新的字段（如 `title`、`category`、`status`），不传入 `clickCount` 字段即可保留原有统计值。系统设计上，点击计数仅由 `POST /api/v1/links/{id}/click` 接口增加，编辑接口不会自动修改该字段。

Q: 内置的链接健康检查机制是否会对目标服务器造成压力？

A: 健康检查模块默认采用 HEAD 请求方式，仅获取响应头信息而不下载完整页面内容，且并发请求数限制为 5 个 / 秒。检查周期默认为每 24 小时一次，可在 `config/schema.js` 中调整 `HEALTH_CHECK_CRON` 与 `HEALTH_CHECK_TIMEOUT` 参数以降低访问频率。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
