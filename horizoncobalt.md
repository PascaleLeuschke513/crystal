# NexusIndex

NexusIndex 是一个面向技术内容创作者、开源项目维护者以及数字资源管理者的高密度外链资源导航与元数据聚合系统。该项目不提供具体的内容托管或文件存储服务，而是专注于对互联网上的公开视频资源、直播平台信息、内容创作者档案以及相关多媒体元数据进行结构化索引与分类展示。其目标用户包括需要对网络公开视频素材进行快速定位的媒体研究人员、需要追踪特定领域内容发布动态的社区运营人员，以及希望构建自有轻量级资源导航站点的开发者。NexusIndex 通过严格的 URL 分类策略、去重校验机制与自动化的健康检查脚本，解决了海量链接难以高效组织、易失效、缺乏上下文标注等核心痛点，为上层应用提供干净、稳定、可扩展的链接数据层。

## 功能概览

- **多维度分类索引**：系统内置了基于内容主题、地域来源、平台属性及更新热度的四级分类标签体系，允许用户按需筛选与排序链接资源，避免单一列表带来的信息过载。

- **自动化可用性探测**：后台守护进程定期对收录的每一枚链接进行 HTTP 状态码校验与响应时间测量，自动标记异常链接并生成告警日志，显著降低人工维护成本。

- **元数据智能补全**：针对每一枚收录链接，系统尝试通过公开 API 或 HTML 元标签抓取页面标题、描述关键词及站点图标，并以结构化 JSON 格式存储，便于前端渲染与检索。

- **灵活的数据导入导出**：支持批量导入 CSV 或 JSON 格式的链接清单，亦可一键导出为纯文本列表、Markdown 表格或 RSS 订阅源，方便与其他工具链集成。

- **轻量级 Web 管理面板**：提供基于 Flask 框架的最小化管理界面，支持链接的增删改查、分类迁移及备注编辑，所有操作均记录操作日志，满足审计需求。

- **可编程 RESTful API**：所有核心功能均通过版本化 API 暴露，支持分页查询、模糊搜索及按字段排序，便于第三方开发者二次开发或嵌入到现有工作流中。

- **部署与运维友好**：项目提供完整的 Docker 容器化方案与 systemd 服务模板，支持一键启动、日志轮转及性能监控，适应从个人开发机到生产服务器的多种环境。

## 应用场景

- **内容聚合站点快速搭建**：个人站长或小团队可利用 NexusIndex 管理数百个外部视频或直播链接，结合分类索引功能，在数小时内构建出一个垂直领域的资源导航子站点，无需从零开发后台管理系统。

- **研究数据采集辅助**：高校或研究机构中从事网络传播学、数字媒体分析的学者，可使用本项目的结构化元数据输出功能，快速获得一批特定主题（如国产网红、日韩直播等）的链接清单，作为后续内容分析或舆情监控的种子数据源。

- **运维自动化巡检**：负责多个社区或平台链接健康度的运维人员，可借助系统内置的探测脚本每日定时执行链接可用性检查，并将异常报告通过邮件或 Webhook 发送至团队群组，确保用户体验。

- **开源项目文档站链接管理**：开源项目维护者可将本项目的链接管理模块嵌入到项目文档站中，用于维护“生态伙伴”、“相关工具”或“媒体素材”等章节的外部引用，保证文档中的链接长期有效且可溯源。

## 快速开始

以下步骤适用于 Linux / macOS 系统，Windows 用户建议使用 WSL2 或 Git Bash 环境。确保已安装 Git、Python 3.9 及以上版本以及 pip。

```bash
# 1. 克隆项目仓库
git clone https://github.com/nexusindex/core.git nexusindex
cd nexusindex

# 2. 创建并激活 Python 虚拟环境
python3 -m venv venv
source venv/bin/activate

# 3. 安装项目依赖（生产与开发依赖）
pip install --upgrade pip
pip install -r requirements.txt
pip install -r requirements-dev.txt

# 4. 初始化本地配置与数据库
cp .env.example .env
python scripts/init_db.py

# 5. 启动开发服务器（默认监听 127.0.0.1:5000）
python app.py
```

访问 `http://127.0.0.1:5000` 即可查看基础前端界面，并通过 `/admin` 路径进入管理面板（默认账号密码见 `.env` 文件说明）。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.9 至 3.12 | 核心运行环境，3.13 暂未完全测试 |
| pip | 21.0 以上 | Python 包管理器，用于安装依赖库 |
| SQLite | 3.35 以上 | 默认嵌入式数据库，无需额外安装；生产环境可切换至 PostgreSQL |
| Redis | 6.0 以上 | 可选，用于缓存 API 响应与任务队列，非必需但强烈推荐 |
| Node.js | 18.x 或 20.x | 仅当需要构建前端静态资源时使用，若仅用后端 API 可忽略 |
| Docker | 20.10 以上 | 若使用容器化部署方式则需要 |
| Git | 2.25 以上 | 用于克隆仓库及版本管理 |
| make | 任意版本 | 辅助运行常用脚本命令（如 make test、make lint） |
| curl | 任意版本 | 用于健康检查脚本中的网络请求，系统通常自带 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | `/docs/user-guide/` | 如何添加新链接、如何分类、如何导入导出数据、前端面板的操作流程 |
| 开发者指南 | `/docs/developer-guide/` | API 鉴权方式、数据模型设计、如何扩展探测器、如何编写自定义过滤器 |
| 运维手册 | `/docs/operations-guide/` | 生产环境部署步骤（Nginx + Gunicorn）、日志切割策略、备份与恢复方案 |
| 架构设计 | `/docs/architecture/` | 系统模块划分、数据流向、并发模型、缓存策略与扩展性考量 |
| 常见问题 | `/docs/faq.md` | 汇总社区高频提问，涵盖安装报错、性能调优、数据迁移等 |
| 版本记录 | `/CHANGELOG.md` | 每个版本的更新摘要、破坏性变更及升级注意事项 |
| 行为准则 | `/CODE_OF_CONDUCT.md` | 社区参与者应遵守的沟通规范与举报流程 |

## 资源列表

本项目的初始样例数据来源于公开的互联网内容平台索引，所有链接均作为演示用途提供，用户可根据自身需求替换或扩展。以下为系统内置的资源分类与具体链接条目：

**视频直播类资源**

<code>https://meinvzhibozaixiankanw.org.cn</code>

<code>https://rihanzhibofulishipinw.org.cn</code>

**国产网红内容类**

<code>https://guochanwanghongfulishipinw.org.cn</code>

**网红热点聚合类**

<code>https://rewuzhibowanghongzhibow.org.cn</code>

<code>https://wanghongmeinvrewuzhibow.org.cn</code>

**无付费网红内容类**

<code>https://wufuyewanghongzhibow.org.cn</code>

<code>https://wufuyemeinvzhibow.org.cn</code>

## 项目结构

```
nexusindex/
├── app/                               # 核心应用包
│   ├── __init__.py                    # 应用工厂函数，初始化 Flask 及扩展
│   ├── routes/                        # 路由蓝图模块
│   │   ├── api_v1.py                  # RESTful API 端点（列表、搜索、详情）
│   │   ├── admin.py                   # 后台管理界面路由
│   │   └── public.py                  # 前端展示页面路由
│   ├── models/                        # 数据模型与 ORM 映射
│   │   ├── link.py                    # 链接实体模型（字段、关系、序列化）
│   │   ├── category.py                # 分类标签模型（树形结构支持）
│   │   └── audit_log.py               # 操作审计日志模型
│   ├── services/                      # 业务逻辑层
│   │   ├── checker.py                 # 链接可用性探测服务（多线程调度）
│   │   ├── metadata.py                # 元数据补全服务（解析 HTML 元标签）
│   │   └── exporter.py                # 数据导出服务（JSON、CSV、RSS）
│   ├── static/                        # 前端静态资源（CSS / JS / 图标）
│   │   ├── css/                       # 基于 Bootstrap 的自定义主题样式
│   │   └── js/                        # 前端交互脚本（列表筛选、批量操作）
│   └── templates/                     # Jinja2 模板文件
│       ├── base.html                  # 基础页面骨架
│       ├── index.html                 # 链接展示主页面
│       └── admin/                     # 管理面板页面集合
├── scripts/                           # 运维及开发辅助脚本
│   ├── init_db.py                     # 初始化数据库表及默认分类数据
│   ├── health_check.py                # 独立运行的链接健康检查脚本（可配合 cron）
│   └── seed_demo.py                   # 导入示例链接数据（含本批次 URL）
├── tests/                             # 单元测试与集成测试
│   ├── conftest.py                    # pytest 配置与共享 fixture
│   ├── test_models.py                 # 数据模型层测试
│   └── test_api.py                    # API 端点响应与状态码测试
├── docs/                              # 项目文档（详见文档导航）
├── .env.example                       # 环境变量模板（含数据库 URI、密钥、调试开关）
├── .gitignore                         # Git 忽略规则（含虚拟环境、缓存、日志）
├── docker-compose.yml                 # 容器编排配置（含 Redis 与可选 PostgreSQL）
├── Dockerfile                         # 生产级镜像构建文件（多阶段构建）
├── Makefile                           # 常用命令快捷方式（install, test, lint, run）
├── README.md                          # 本文件
├── requirements.txt                   # 生产依赖（Flask, SQLAlchemy, requests 等）
└── requirements-dev.txt               # 开发依赖（pytest, black, flake8 等）
```

## 贡献指南

我们欢迎社区贡献，无论是报告问题、提交代码还是完善文档。请遵循以下流程以确保协作顺畅：

1.  **查阅现有议题**：在提交新议题前，请先浏览 GitHub Issues 列表，确认该问题或建议尚未被他人提出。若存在相似议题，可在下方补充信息而非重复创建。

2.  **Fork 仓库并创建特性分支**：从主仓库的 `main` 分支 fork 到个人账户，然后在本地创建描述性的分支名称，例如 `feature/add-csv-importer` 或 `fix/checker-timeout`。

3.  **编写代码与测试**：任何功能性代码变更必须附带对应的单元测试，测试覆盖率不应低于原有水平。请确保代码通过 `make lint` 与 `make test` 检查，并遵循项目约定的命名规范（PEP 8 与 Flask 最佳实践）。

4.  **提交变更与签署开发者源证书**：提交信息应清晰描述变更动机与实现方式。所有提交必须附带 Signed-off-by 行（使用 `git commit -s`），以表明您同意开发者源证书（Developer Certificate of Origin）。

5.  **发起 Pull Request**：向 `main` 分支发起 PR，并在描述中关联相关议题编号。PR 模板已包含检查清单，请逐项确认。维护者将在 2 个工作日内进行评审，可能要求修改或补充。

## 常见问题

**Q：系统启动后，管理面板无法登录，提示“Invalid credentials”。**
A：请检查项目根目录下的 `.env` 文件是否已正确配置 `ADMIN_USERNAME` 和 `ADMIN_PASSWORD` 变量。若未设置，系统将回退到默认凭据（admin / changeme），但强烈建议生产环境中修改。您也可以运行 `python scripts/reset_admin.py` 脚本强制重置管理员密码。

**Q：健康检查脚本报告大量链接超时，但浏览器可以正常访问。**
A：这是因为脚本默认使用无头 HTTP 客户端，且超时时间设置为 3 秒。某些站点可能对非浏览器 User-Agent 或快速连续请求有限流策略。您可以通过修改 `.env` 中的 `CHECKER_TIMEOUT` 和 `CHECKER_USER_AGENT` 变量调整参数。若问题依然存在，请考虑启用代理支持（配置 `HTTP_PROXY` 环境变量）。

**Q：如何将 SQLite 迁移到 PostgreSQL 用于生产环境？**
A：项目使用 SQLAlchemy ORM，迁移过程较为平滑。首先在 PostgreSQL 中创建空数据库，然后修改 `.env` 中的 `DATABASE_URL` 为 `postgresql://user:pass@host/dbname`。接着运行 `python scripts/migrate_db.py --target postgres`，该脚本会自动读取当前 SQLite 数据并导入到 PostgreSQL。迁移完成后，重启应用即可。

## 许可证

MIT License

Copyright (c) 2026 NexusIndex Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
