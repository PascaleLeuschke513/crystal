# OpenWatch 媒体资源索引系统

OpenWatch 是一个面向视频内容研究者、媒体分析师及开发者的高可用媒体资源元数据索引与导航系统。本项目不存储、不托管、不分发任何音视频文件，仅对公开互联网上可访问的媒体资源网站进行结构化信息整理与可用性探测，解决媒体资源发现过程中信息碎片化、链接失效、来源不可溯等核心问题。项目目标用户包括学术机构媒体观察员、内容合规审计人员、以及需要长期追踪多源媒体动态的技术开发者。

## 功能概览

- **多源资源聚合索引**：对用户提交及系统发现的媒体资源站点进行统一编目，提取站点标题、服务类型、内容语言、地理归属等元数据。
- **可用性健康检查**：定期对索引库中的资源链接执行HTTP可用性探测，自动标记异常状态（如403、DNS解析失败），并生成可用性报表。
- **分类标签系统**：支持按地区（欧美、日韩、华语）、内容类型（影视、短视频、纪录片）、语种（中文字幕、配音版）等维度进行多级标签筛选。
- **自定义资源看板**：允许用户创建私有的资源分组，针对特定研究项目或合规任务建立独立的关注列表，并支持看板数据导出为CSV/JSON。
- **变更通知订阅**：当被监控的资源站点发生状态变更（如新增、下线、响应码异常）时，系统通过Webhook或邮件队列向订阅者发送结构化告警。
- **访问日志分析**：提供轻量级访问统计功能，记录资源链接的被请求次数、最后访问时间，辅助分析资源热度趋势。
- **开放API接口**：所有索引数据均通过RESTful API对外开放，支持第三方工具批量拉取、交叉验证或集成至更高级的数据处理流水线。

## 应用场景

- **媒体合规性监测**：合规团队可利用本系统持续追踪指定名单上的媒体资源站点可用性，当某站点响应异常时第一时间触发复核流程，确保合规审查样本的完整性。
- **学术研究数据采集**：传播学或社会学研究者通过系统提供的分类过滤与API接口，可快速获取特定区域或主题的媒体资源分布清单，为内容分析研究提供样本框架。
- **DevOps依赖监控**：负责内容聚合或推荐系统的开发团队，可将本系统作为上游数据源的健康检查前置层，在自身业务请求前预判外部资源可访问性，从而优化请求调度策略。
- **个人媒体库管理**：高级用户可利用看板功能对个人关注的零散影视资源网站进行集中收藏与状态跟踪，替代浏览器书签中杂乱且无法感知可用性的传统URL收藏方式。

## 快速开始

以下步骤帮助您在本地环境快速部署并运行OpenWatch索引服务的开发实例。

```bash
# 1. 克隆项目仓库至本地
git clone https://github.com/openwatch/openwatch-indexer.git
cd openwatch-indexer

# 2. 安装项目依赖（使用Python虚拟环境）
python -m venv venv
source venv/bin/activate  # Windows系统请使用 venv\Scripts\activate
pip install -r requirements.txt

# 3. 初始化本地SQLite数据库并加载示例资源数据
python manage.py initdb
python manage.py load-fixtures --source resources/fixtures/seed.json

# 4. 启动开发服务器
python manage.py runserver --host 0.0.0.0 --port 8080

# 服务启动后，可通过浏览器访问 http://localhost:8080 查看仪表盘
# API文档访问 http://localhost:8080/api/docs
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.9 - 3.11 | 核心运行时环境，建议使用3.10稳定版 |
| SQLite | 3.35+ | 默认嵌入式数据库，用于开发及小规模部署 |
| Redis | 6.2+ | 可选组件，用于生产环境下的缓存与任务队列（Celery） |
| Node.js | 16.0+ | 仅用于前端静态资源构建（非运行必需） |
| Docker | 20.10+ | 可选，用于容器化部署方案 |
| Nginx | 1.20+ | 生产环境反向代理与静态文件服务推荐 |
| Celery | 5.2+ | 异步任务处理器，用于执行定时可用性探测 |
| Flower | 1.0+ | 可选，Celery任务监控面板 |

## 文档导航

| 层面 | 目录/文档 | 回答的问题 |
|---|---|---|
| 用户手册 | `/docs/user-guide/` | 如何注册账号、创建资源看板、配置订阅规则、使用分类筛选？ |
| 开发者指南 | `/docs/developer/` | API鉴权方式、数据模型字段说明、如何编写自定义探测插件？ |
| 运维手册 | `/docs/ops/` | 生产环境部署架构、环境变量配置、健康检查端点说明、日志采集规范 |
| 架构设计 | `/docs/architecture/` | 系统模块划分、数据流图、可用性探测调度算法、缓存更新策略 |
| 贡献规范 | `/CONTRIBUTING.md` | 代码提交格式、分支命名规则、测试覆盖率要求、Pull Request流程 |
| 安全策略 | `/SECURITY.md` | 漏洞报告渠道、敏感信息脱敏规范、访问控制权限模型 |

## 资源列表

本系统索引的媒体资源站点原始列表（由用户提交）如下。所有链接均保持原始格式，系统仅做元数据记录与可用性探测。

**影视综合类**
- <code>https://zhongwenzimuzaixianyingshiyuan.org.cn</code>
- <code>https://mianfeiguankanzaixianguankan.org.cn</code>

**短视频与综合视频**
- <code>https://jiujiushipinzaixianguankan.org.cn</code>
- <code>https://mianfeigaoqingshipinzaixianguankan.org.cn</code>

**地区分类**
- <code>https://oumeizaixianguankanshipin.org.cn</code>
- <code>https://rihanshipinmianfeizaixianguankan.org.cn</code>

**字幕与语言辅助**
- <code>https://renqixiliezhongwenzimuw.org.cn</code>

## 项目结构

项目采用分层架构，各模块职责清晰，便于维护与扩展。

```
openwatch-indexer/
├── src/                                 # 核心源代码目录
│   ├── core/                           # 核心配置与启动模块
│   │   ├── settings.py                # 全局配置（数据库、缓存、队列）
│   │   └── celery_app.py              # Celery应用实例定义
│   ├── indexer/                       # 资源索引引擎
│   │   ├── crawler/                   # 元数据抓取与解析器
│   │   ├── classifier/                # 标签分类与自动打标模块
│   │   └── serializer/                # 索引数据序列化（JSON/XML）
│   ├── probe/                         # 可用性探测模块
│   │   ├── checker.py                 # HTTP/HTTPS健康检查逻辑
│   │   ├── scheduler.py               # 定时任务调度（基于APScheduler）
│   │   └── notifier.py                # 状态变更通知分发器
│   ├── api/                           # RESTful API层
│   │   ├── v1/                        # API版本1端点
│   │   │   ├── resources.py           # 资源CRUD接口
│   │   │   └── dashboard.py           # 看板数据聚合接口
│   │   └── auth.py                    # JWT鉴权实现
│   └── web/                           # Web UI控制台
│       ├── static/                    # 编译后的前端静态资源
│       └── templates/                 # 服务端渲染模板（基础页）
├── tests/                              # 单元测试与集成测试
│   ├── unit/                          # 各模块单元测试
│   └── fixtures/                      # 测试用模拟数据
├── scripts/                            # 运维与辅助脚本
│   ├── initdb.py                      # 数据库初始化
│   └── load_fixtures.py               # 测试数据加载
├── docs/                               # 完整项目文档（详见文档导航）
├── requirements.txt                    # Python生产依赖列表
├── requirements-dev.txt                # 开发及测试额外依赖
├── Dockerfile                          # 容器镜像构建定义
├── docker-compose.yml                  # 本地开发容器编排
└── README.md                           # 项目入口说明（本文档）
```

## 贡献指南

我们欢迎任何形式的贡献，包括但不限于新增资源索引源、优化探测算法、改进文档或提交缺陷修复。

1.  **问题讨论与任务认领**：在提交代码前，请先在GitHub Issues中搜索是否存在相关话题。若无，请新建一个Issue详细描述您想要解决的问题或功能提议，并等待核心维护者的反馈与标记。
2.  **派生仓库并创建功能分支**：将本项目派生至您的个人GitHub账户，然后基于`develop`分支创建一个新的功能分支。分支命名请遵循`feature/简要描述`或`fix/问题编号`的格式。
3.  **编写代码与测试**：请确保您的代码符合项目PEP8编码规范，并为新增功能或修复编写相应的单元测试，确保所有测试用例通过（执行`pytest tests/`）。
4.  **提交变更并推送**：提交信息请采用语义化格式，首行简明概括变更内容。推送至您的派生仓库后，通过GitHub界面发起一个向`develop`分支的Pull Request。
5.  **代码审查与合并**：核心维护者将对您的Pull Request进行审查，可能提出修改意见。通过审查后，您的代码将被合并至主分支，并出现在下一个版本中。

## 常见问题

**问：系统提示某个资源链接返回403状态码，这是什么原因？**

答：403状态码通常表示目标服务器的访问控制策略拒绝了我们的探测请求。这可能是由于服务器配置了反爬虫机制、要求特定的User-Agent头部、或基于IP地址的访问限制。系统会在探测日志中记录详细的响应头部信息。您可以在资源详情页手动触发一次“深度探测”，该操作会模拟不同浏览器标识并尝试多种协议版本。若长期不可用，建议从看板中移除该链接或联系该站点的服务提供方。

**问：如何确保我的订阅数据不会丢失，或者看板配置能长期保存？**

答：在生产部署中，我们推荐使用PostgreSQL或MySQL作为后端数据库，并配置定期的自动备份策略。所有用户看板和订阅配置均持久化存储于数据库事务日志中。若使用默认的SQLite数据库，请手动备份`data/`目录下的数据库文件。系统本身不存储任何媒体内容，仅保存元数据与状态记录，因此备份数据量通常较小，建议每日通过`scripts/backup.py`脚本进行增量备份。

**问：我能否将OpenWatch作为其他应用的依赖库（Library）使用，而不启动完整的Web服务？**

答：可以。项目的核心索引与探测模块已设计为独立Python包。您可以通过`pip install -e ./src/core`的方式仅安装核心模块，然后在您的自定义脚本中直接导入`from indexer import crawler`或`from probe import checker`等接口。具体API使用方法请参阅开发者指南中的“核心模块编程接口”章节。

## 许可证

本项目采用MIT许可证进行分发。您可以在遵守许可证条款的前提下自由使用、修改、分发本软件，包括用于商业目的。详细条款请参阅项目根目录下的LICENSE文件。

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
