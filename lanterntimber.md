# WebStream 资源导航系统

WebStream 是一个面向技术内容创作者与流媒体资源研究者的外链导航与元数据聚合系统。本项目不对任何外部资源进行存储、分发或代理，仅提供公开可访问的 URL 索引与分类整理功能，帮助用户快速定位特定类型的网络直播与视频内容入口。系统核心定位为「只收录、不触碰」，所有跳转均直连原始域名，不经过中间代理或重写。

目标用户包括：网络内容分类研究者、流媒体平台数据分析人员、合规性审查辅助工具开发者，以及需要批量管理外链资源的运维工程师。WebStream 通过结构化的目录树、标签化分类与一键复制功能，将散落于互联网各处的直播入口链接整合为可维护、可审计的本地知识库，大幅降低人工收藏夹的管理成本与检索耗时。

## 功能概览

- **外链分类索引**：按内容主题自动分组，支持自定义标签体系，每条链接可关联多个分类标签，便于多维度筛选。

- **批量导入与导出**：支持 CSV 与 JSON 格式的链接批量导入，同时可导出为 Markdown 表格或纯文本列表，便于迁移与备份。

- **链接可用性检测**：定时对已收录 URL 发送 HEAD 请求，检测响应状态码与页面标题变更，异常链接自动标记并生成告警日志。

- **自定义元数据字段**：用户可为每条链接添加备注、评分、收录日期、最后验证时间、所属地区等扩展字段，满足个性化管理需求。

- **快速检索与过滤**：基于标题、域名、标签、备注内容的全文搜索，支持正则表达式过滤与多条件组合筛选。

- **目录树可视化导航**：左侧目录树与右侧内容面板联动，点击目录节点即时刷新链接列表，操作路径清晰可循。

- **权限分级控制**：支持多用户只读与可写权限划分，适合团队协作场景，避免误删或误改核心资源。

## 应用场景

- **流媒体内容分类研究**：研究人员可将不同主题的直播入口按「舞蹈」「户外」「游戏」「娱乐」等标签归类，定期分析各分类下的域名分布与存活率，为行业报告提供数据支撑。

- **合规性审查辅助**：企业合规部门可利用本系统建立内部审查清单，将待审的外链统一收录并周期性检测其内容变更情况，及时标记风险域名。

- **个人知识库构建**：内容运营人员可将日常积累的直播平台、博主主页、资源站点汇总至 WebStream，配合备注字段记录平台特点、受众规模等信息，形成个人专属的资源地图。

- **运维监控仪表板**：运维工程师可将系统部署至内网服务器，配合定时任务自动检测所有外链的可用性，通过邮件或 Webhook 发送异常告警，保障业务依赖的外部资源稳定可达。

## 快速开始

以下步骤适用于 Linux / macOS / Windows WSL 环境，确保系统已安装 Git、Node.js 18+ 与 npm。

```bash
# 1. 克隆代码仓库
git clone https://github.com/webstream-org/webstream-navigator.git
cd webstream-navigator

# 2. 安装项目依赖
npm install

# 3. 启动开发服务器
npm run dev
```

执行完毕后，终端会输出本地访问地址（默认 http://localhost:5173），打开浏览器即可进入 WebStream 主界面。首次启动将自动生成示例数据文件于 `./data/sample.json`，用户可基于此模板开始录入自己的链接资源。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Node.js | 18.x 或 20.x LTS | 运行时环境，用于执行构建脚本与开发服务器 |
| npm | 9.x 或更高 | 包管理器，用于安装项目依赖 |
| Git | 2.30 或更高 | 版本控制工具，用于克隆仓库与提交变更 |
| 磁盘空间 | 至少 200 MB | 存放源代码、依赖包及本地数据库文件 |
| 内存 | 至少 512 MB | 开发模式运行所需最低内存，生产模式建议 1 GB 以上 |
| 操作系统 | Linux / macOS / Windows 10+ (含 WSL2) | 跨平台支持，Windows 原生需配合 PowerShell 7+ |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户手册 | /docs/user-guide/ | 如何添加链接、创建分类、执行搜索与导出数据？ |
| 管理员指南 | /docs/admin-guide/ | 如何配置定时检测、管理用户权限、备份数据？ |
| 开发参考 | /docs/development/ | 项目目录结构说明、核心 API 接口定义与二次开发流程？ |
| 部署运维 | /docs/deployment/ | 如何将系统部署至生产服务器（Nginx + PM2 或 Docker）？ |
| 常见问题 | /docs/faq/ | 检测失败的原因有哪些？如何迁移旧版数据？ |

## 资源列表

本系统收录的所有外部链接均整理如下，按内容主题划分为三个子类，便于用户按需查阅。每条 URL 均严格保留用户所提供的原始格式，未做任何协议补全、域名改写或大小写调整。

**综合直播分类**

<code>https://wufuyewanghongzhibow.org.cn</code>

<code>https://wanghongzhibofulizaixianw.org.cn</code>

**垂直内容分类**

<code>https://wufuyemeinvzhibow.org.cn</code>

<code>https://meinvwufuyiezhibow.org.cn</code>

<code>https://nvzhubozshipinzaixianguankanw.org.cn</code>

**特色内容分类**

<code>https://shuaigefujifulizhibow.org.cn</code>

<code>https://oubazhibomianfeiguankanw.org.cn</code>

## 项目结构

```
webstream-navigator/
├── public/                         # 静态资源目录
│   ├── favicon.ico                 # 网站图标
│   └── robots.txt                  # 搜索引擎爬虫规则
├── src/                            # 源代码主目录
│   ├── api/                        # 后端接口层
│   │   ├── routes/                 # 路由定义文件
│   │   └── middleware/             # 鉴权、日志等中间件
│   ├── core/                       # 核心业务逻辑
│   │   ├── link-manager.js         # 链接增删改查与分类引擎
│   │   ├── health-checker.js       # 定时可用性检测调度器
│   │   └── data-importer.js        # CSV/JSON 批量导入处理器
│   ├── ui/                         # 前端界面组件
│   │   ├── pages/                  # 页面级 Vue/React 组件
│   │   ├── components/             # 可复用 UI 部件
│   │   └── styles/                 # 全局样式表与主题变量
│   ├── utils/                      # 工具函数集合
│   │   ├── url-parser.js           # URL 解析与格式化工具
│   │   └── logger.js               # 统一日志输出模块
│   └── config/                     # 配置文件目录
│       ├── default.json            # 默认配置项
│       └── custom.json             # 用户自定义配置（不提交至仓库）
├── data/                           # 本地数据存储目录
│   ├── links.db                    # SQLite 数据库文件
│   └── backups/                    # 自动备份存档
├── tests/                          # 单元测试与集成测试
│   ├── unit/                       # 单元测试用例
│   └── integration/                # 接口联调测试
├── docs/                           # 完整文档源码
├── scripts/                        # 运维与构建辅助脚本
├── package.json                    # npm 项目清单
├── README.md                       # 本文件
└── LICENSE                         # MIT 许可证文本
```

## 贡献指南

1. 复刻主仓库至个人账户，并在本地创建功能分支（如 `feat/add-custom-tag-filter`），所有开发工作均在该分支上进行。

2. 遵循项目代码风格（ESLint + Prettier 配置已内置），提交前执行 `npm run lint` 与 `npm run test` 确保无语法错误与测试失败。

3. 为新增功能或修复补丁编写对应的单元测试用例，测试覆盖率不低于现有基线（80%），并在 Pull Request 描述中说明变更动机与影响范围。

4. 若涉及外部链接分类规则的调整，需同步更新 `./docs/user-guide/classification.md` 文档，确保帮助文档与代码逻辑保持一致。

5. 提交 Pull Request 后，等待至少一位维护者进行 Code Review，根据反馈意见修改直至通过合并检查。

## 常见问题

**问：可用性检测返回 403 或 429 状态码，是否表示链接已失效？**

不一定。部分外部站点会针对自动化请求（如 HEAD 请求）返回拒绝访问或限流响应，此情形下系统会标记为「疑似受限」而非「失效」。建议用户结合备注字段手动复核，或调整检测间隔与 User-Agent 配置。

**问：如何将现有浏览器收藏夹批量导入至 WebStream？**

目前支持从 Chrome / Firefox 导出的 HTML 书签文件（需先转换为 CSV 格式），以及任意符合模板字段的 JSON 数组。具体操作请参考 `/docs/user-guide/batch-import.md` 中的步骤说明，模板文件位于 `./data/import-template.json`。

**问：数据存储是否支持外部数据库（如 PostgreSQL）？**

当前版本默认使用嵌入式 SQLite 数据库，适合单机或小规模团队使用。若需对接外部数据库，可修改 `./src/config/custom.json` 中的 `datastore` 配置段，并安装对应驱动包，详细配置参数见 `/docs/admin-guide/external-db.md`。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
