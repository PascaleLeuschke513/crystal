# LinkPilot 资源导航引擎

LinkPilot 是一个面向技术社区与内容创作者的轻量级外链资源导航与聚合平台。项目定位为“技术化书签管理工具”，主要服务于需要高频访问、分类管理与快速检索外部优质资源的开发者、研究员及运维人员。通过标签化索引、状态监控与简易分享机制，解决个人与团队在分散网络资源中查找效率低、链接失效难以及时发现、资源贡献与协作缺乏统一入口的核心痛点。

项目采用静态站点生成逻辑与动态链路巡检相结合的设计思路，核心数据基于 Markdown 与 YAML 维护，无需重型数据库依赖，适合在低资源环境下快速部署。输出为结构化 Web 页面或 API 数据流，便于嵌入现有仪表盘、内网门户或知识库系统。

## 功能概览

- **多级分类索引**：支持无限层级的目录与标签体系，允许为每个外链资源标记多个属性标签，实现精确筛选与模糊检索。
- **链接健康状态巡检**：内置定时任务模块，可对收录的 URL 进行 HTTP 状态码检查，自动标记异常链接（如 403、404、超时），并生成巡检报告。
- **资源元数据管理**：每条记录支持标题、描述、分类、标签、添加时间、最后验证时间、责任人等多维度元数据字段，便于审计与质量追踪。
- **快速提交与审核流**：提供简易表单接口或命令行工具，允许团队成员提交新链接，并支持管理员审核后发布，保证资源质量。
- **视图模板化输出**：支持列表视图、卡片视图、JSON API 视图等多种展示模式，便于适配不同终端（PC、移动端、命令行工具）。
- **导入导出兼容性**：支持从常见书签文件（HTML 书签导出、JSON 格式）批量导入资源，同时支持将导航数据导出为 CSV 或 Markdown 表格，用于离线备份或迁移。
- **访问统计与热度排序**：记录每个链接的点击次数，支持按热度、添加时间、名称排序，帮助用户快速定位高频资源。

## 应用场景

- **技术团队内部知识库导航**：开发团队可将常用的 API 文档、设计规范、CI/CD 工具链地址、日志平台入口等统一收录至 LinkPilot，通过标签区分环境（生产/测试/预发布），避免重复查询和误用。
- **个人开发者的资源工作台**：独立开发者或研究员可将关注的开源项目、论文数据库、数据可视化工具、云服务控制台等分门别类整理，配合巡检功能每日自动检查可用性，减少手动确认时间。
- **社区共建资源目录**：技术社区或开源社区可利用 LinkPilot 的提交与审核机制，共同维护一份“精选工具与库”清单，定期更新并对外发布为静态页面，降低新成员的学习曲线。
- **活动或课程资料汇总**：举办黑客松、技术培训或在线课程时，可将涉及的所有平台链接、示例代码仓库、幻灯片地址、实时看板等集中在一个导航项目中，活动结束后可归档为历史版本。
- **运维监控仪表盘辅助**：运维人员可将内部监控系统、日志查询界面、报警管理后台等入口统一配置，结合巡检功能快速发现因证书过期或策略变更导致的访问异常。

## 快速开始

以下步骤将指导您在本地环境快速启动 LinkPilot 实例。

```bash
# 1. 克隆项目仓库
git clone https://github.com/linkpilot-dev/linkpilot-core.git
cd linkpilot-core

# 2. 安装依赖（基于 Python 3.9+ 与 pip）
pip install -r requirements.txt

# 3. 初始化配置文件与环境变量
cp .env.example .env
# 根据注释修改 .env 中的巡检间隔、存储路径等参数

# 4. 导入示例资源数据
python manage.py import --source ./data/sample_bookmarks.json

# 5. 启动开发服务器
python manage.py runserver --port 8080

# 访问 http://localhost:8080 查看导航界面
# 访问 http://localhost:8080/api/v1/links 获取 JSON 格式资源列表
```

生产环境部署建议使用 Gunicorn + Nginx，具体参考 `deploy` 目录下的示例配置。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.9 至 3.11 | 核心运行环境，推荐使用 3.10 以获得最佳兼容性 |
| pip | 21.0 以上 | Python 包管理工具，用于安装项目依赖 |
| SQLite | 3.28 以上 | 默认内置数据库，用于存储链接元数据与巡检记录，生产环境可切换至 PostgreSQL |
| Redis | 6.0 以上 | 可选，用于缓存高频访问的 API 数据与分布式锁，非必需但强烈推荐 |
| Node.js | 16.0 以上 | 仅当启用前端资产构建功能时需要，用于编译 CSS 与 JavaScript 资源 |
| Git | 2.25 以上 | 用于版本管理与通过 Git 协议导入部分外部资源元数据 |

## 文档导航

| 层面 | 目录/文档 | 回答的问题 |
|------|-----------|------------|
| 用户手册 | `docs/user-guide/quick-start.md` | 如何安装、配置与启动服务？如何添加第一条链接？ |
| 用户手册 | `docs/user-guide/advanced-search.md` | 如何使用组合标签、时间范围与状态筛选进行高级检索？ |
| 管理员指南 | `docs/admin/health-check.md` | 巡检机制如何工作？如何调整巡检频率与通知方式？ |
| 管理员指南 | `docs/admin/import-export.md` | 如何批量导入浏览器书签？如何备份或迁移整个导航库？ |
| 开发者文档 | `docs/developer/api-reference.md` | 提供了哪些 RESTful API？请求与响应的数据结构是什么？ |
| 开发者文档 | `docs/developer/contributing.md` | 代码风格要求、测试流程与提交规范是什么？ |
| 架构设计 | `docs/architecture/data-model.md` | 核心数据表结构、字段含义与索引策略如何设计？ |

## 资源列表

本导航项目预置并推荐以下优质外链资源，按类别分组展示。所有链接均取自用户原始数据，未经修改。

通用直播与视频内容聚合

<code>https://wanghongzhibofulizaixianw.org.cn</code>

<code>https://nvzhubozshipinzaixianguankanw.org.cn</code>

<code>https://xingganmeinvzhibotiaowuw.org.cn</code>

<code>https://hanguomeinvzhuborewuw.org.cn</code>

<code>https://zaixianbofangzhubow.org.cn</code>

<code>https://zhubozhibozaixianguankanw.org.cn</code>

体育数据与实时比分参考

<code>https://zuqiujishibifend.org.cn</code>

> 注：部分链接在巡检时可能返回 HTTP 403 状态码，这通常表示目标站点启用了访问控制（如防盗链、IP 白名单或用户代理校验）。建议通过浏览器直接访问以确认资源有效性，或参考文档中的“自定义请求头”配置章节调整巡检策略。

## 项目结构

```bash
linkpilot-core/
├── app/                           # 核心应用主目录
│   ├── api/                       # RESTful API 路由与视图函数
│   │   ├── v1/                    # 当前稳定版本 API
│   │   │   ├── links.py           # 链接资源 CRUD 接口
│   │   │   └── health.py          # 巡检状态查询与触发接口
│   │   └── __init__.py
│   ├── models/                    # 数据模型定义 (SQLAlchemy ORM)
│   │   ├── link.py                # 链接实体模型，含 URL、标题、标签、状态等字段
│   │   ├── tag.py                 # 标签模型，用于多对多关联
│   │   └── audit_log.py           # 操作审计日志模型
│   ├── services/                  # 业务逻辑服务层
│   │   ├── checker.py             # 链接健康度巡检调度与执行逻辑
│   │   ├── importer.py            # 多种格式（JSON/HTML）导入处理器
│   │   └── stats.py               # 点击统计与热度计算服务
│   ├── templates/                 # 服务端渲染页面模板 (Jinja2)
│   │   ├── base.html              # 基础布局模板
│   │   ├── index.html             # 导航首页，展示分类与搜索框
│   │   └── detail.html            # 单个链接详情与历史状态页面
│   └── static/                    # 静态资源 (CSS/JS/图片)
│       ├── css/                   # 基于 Tailwind CSS 定制样式
│       └── js/                    # 前端交互脚本，含搜索防抖与状态提示
├── config/                        # 配置管理目录
│   ├── settings.py                # 基础配置 (数据库连接、巡检间隔、缓存开关)
│   ├── .env.example               # 环境变量示例文件
│   └── logging.conf               # 日志格式与输出级别配置
├── data/                          # 数据存储目录
│   ├── sqlite.db                  # 默认 SQLite 数据库文件 (生产环境勿用)
│   └── sample_bookmarks.json      # 示例资源数据，用于快速导入演示
├── tests/                         # 单元测试与集成测试目录
│   ├── test_checker.py            # 巡检模块测试用例
│   ├── test_models.py             # ORM 模型测试
│   └── conftest.py                # pytest 全局夹具与配置
├── scripts/                       # 运维与开发辅助脚本
│   ├── migrate_db.py              # 数据库迁移脚本 (基于 Alembic)
│   └── seed_data.py               # 生成测试数据
├── deploy/                        # 部署相关配置
│   ├── docker-compose.yml         # Docker Compose 编排 (含 Redis 与 PostgreSQL)
│   └── nginx.conf                 # 示例 Nginx 反向代理配置
├── requirements.txt               # Python 依赖清单
├── setup.py                       # 项目安装脚本 (用于 pip install -e .)
└── README.md                      # 当前文档
```

## 贡献指南

我们欢迎并感谢任何形式的贡献，包括但不限于问题报告、功能建议、代码提交与文档完善。请遵循以下流程：

1.  **报告问题或提出新功能**：在 GitHub Issues 页面选择对应模板，清晰描述遇到的问题或期望的功能，并提供复现步骤或使用场景说明。建议先搜索已有 Issue，避免重复。
2.  **Fork 仓库并创建特性分支**：从主仓库 Fork 至个人账户，然后基于 `main` 分支创建新的分支，分支命名建议遵循 `feature/功能简述` 或 `fix/问题简述` 格式。
3.  **本地开发与自测**：确保代码符合 PEP 8 风格规范，并为新增功能或修复编写相应的单元测试。运行 `pytest` 确保所有测试用例通过且不降低覆盖率。
4.  **提交代码并签署 DCO**：提交信息应简洁明了，使用英文或中文均可。提交时需签署 Developer Certificate of Origin (DCO)，即通过 `git commit -s` 提交，证明您有权贡献该代码。
5.  **发起 Pull Request**：将本地分支推送至您 Fork 的仓库，然后向主仓库的 `main` 分支发起 Pull Request。PR 描述中请关联相关 Issue，并等待维护者评审。评审通过后，您的贡献将被合并。

## 常见问题

**Q: 启动服务时提示 “Port already in use”，如何解决？**

A: 这表示默认端口 8080 已被其他进程占用。您可以通过以下方式解决：
1. 在启动命令中指定其他端口，例如 `python manage.py runserver --port 8081`。
2. 查找并终止占用端口的进程：在 Linux/macOS 下使用 `lsof -i:8080` 查看 PID，然后 `kill -9 PID`；在 Windows 下使用 `netstat -ano | findstr :8080` 查看 PID，然后 `taskkill /PID PID /F`。

**Q: 巡检功能显示大量 403 状态码，是否表示链接全部失效？**

A: 不一定。HTTP 403 通常表示目标服务器理解请求但拒绝执行，这可能是由于目标站点启用了反爬机制（如检查 User-Agent 或 Referer）。LinkPilot 默认使用 Python `requests` 库的默认 User-Agent。您可以在配置文件中修改 `CHECKER_USER_AGENT` 为常见浏览器的 User-Agent 字符串，或在巡检配置中允许携带目标站点所需的 Cookie 头。同时，建议通过浏览器直接访问确认资源是否实际可用。

**Q: 如何从浏览器书签批量迁移到 LinkPilot？**

A: 您可以从浏览器（如 Chrome、Firefox）导出书签为 HTML 文件。然后使用 LinkPilot 提供的导入命令：`python manage.py import --source /path/to/bookmarks.html --format html`。系统会自动解析书签文件夹结构为标签，并保留标题与 URL。导入完成后，您可在 Web 界面中进一步整理和补充描述。

## 许可证

本项目采用 MIT 许可证。您被允许自由使用、修改、复制、分发本项目代码，无论用于商业或非商业目的，但需保留原始版权声明与许可声明。详见项目根目录下的 `LICENSE` 文件。

> 外链数量: 7 | 生成时间: 2026-08-14 22:08:29
