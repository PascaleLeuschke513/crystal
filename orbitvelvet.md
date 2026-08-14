# LinkHub 技术导航站

LinkHub 是一个面向开发者和技术研究人员的轻量级外链资源聚合与导航平台。项目定位为技术社区驱动的垂直领域信息枢纽，通过人工筛选与自动化健康检查相结合的方式，对特定行业的数据接口、比分直播、实时统计等类型的外部资源进行结构化整理与稳定化托管。

目标用户包括量化分析爱好者、体育数据研究开发者、开源情报分析人员以及需要快速接入第三方公开数据接口的工程团队。项目本身不存储任何原始数据，仅提供经过可用性验证的 URL 索引与访问链路优化，解决用户在信息检索中面临的高延迟、链接失效、来源不可靠等核心痛点。

## 功能概览

- **实时资源可用性探测**：系统每十分钟对收录的全部外链执行一次 HEAD 请求与响应超时检查，自动标记异常节点并延迟调度。

- **分类标签与多级筛选**：所有资源按行业领域、数据粒度、更新频率三个维度打标，支持组合条件快速过滤。

- **访问链路性能统计**：记录每个外部资源在中国大陆多个运营商的平均响应时间，提供历史可用性趋势图表。

- **自定义收藏分组**：用户可创建私有分组将常用资源归集，支持分组级别的批量导出为 JSON 或 CSV 格式。

- **每日变更日志推送**：记录新增、下架、URL 变更等操作，通过 RSS 或 Webhook 方式通知订阅者。

- **对外只读 API 接口**：提供 RESTful 风格的资源查询接口，支持按标签、关键字、响应时间排序等方式获取资源列表，便于第三方集成。

- **管理后台一键刷新**：授权管理员可手动触发全量或增量资源刷新，重新执行健康检查并更新缓存状态。

## 应用场景

- 体育数据看板开发者在搭建实时比分展示模块时，可通过 LinkHub 快速定位多个备用数据源，当主数据源响应超时时自动切换至备用来源，保障看板稳定性。

- 量化策略研究团队需要从多个公开接口获取联赛积分与赛果数据进行回测建模，LinkHub 提供统一的入口索引和状态监控，减少人工维护链接列表的负担。

- 开源情报分析人员在使用境外公开数据时面临访问波动问题，LinkHub 每日健康检查日志可辅助判断哪些资源在特定时段更稳定，辅助选择最优采集窗口。

- 技术博客作者或社区维护者在撰写相关领域教程时，可直接引用 LinkHub 中的资源分类列表，避免教程中的外链过早失效，延长内容有效周期。

## 快速开始

以下步骤适用于 Linux / macOS 环境，Windows 用户建议通过 WSL2 或 Git Bash 执行。

```bash
# 克隆项目仓库
git clone https://github.com/linkhub-dev/linkhub.git
cd linkhub

# 安装项目依赖（使用 pip 与虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 初始化本地 SQLite 数据库并加载示例资源
python manage.py init_db
python manage.py load_seed_data

# 启动开发服务器（默认监听 127.0.0.1:8080）
python manage.py runserver
```

访问本地 8080 端口即可看到导航站首页，初始管理员账号为 `admin`，密码在首次启动时输出于终端日志中。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.10 及以上 | 核心运行环境，低于此版本将导致类型注解解析异常 |
| pip | 22.0 及以上 | 依赖包管理工具，用于安装 requirements.txt 中声明的库 |
| SQLite | 3.35 及以上 | 内置数据库，用于存储资源索引、标签及健康检查历史记录 |
| requests | 2.28.0 及以上 | 发送 HTTP 探测请求，依赖系统 CA 证书链完整性 |
| Flask | 2.2.0 及以上 | Web 框架，提供路由控制与模板渲染能力 |
| APScheduler | 3.10.0 及以上 | 定时任务调度器，驱动后台健康检查循环 |
| gunicorn | 20.1.0 及以上 | 生产环境 WSGI 服务器，仅在部署时依赖（开发环境可选） |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | /docs/quickstart.md | 如何在一分钟内完成初次配置并看到首页效果？ |
| 运维手册 | /docs/operations.md | 如何调整健康检查频率、更换数据库或迁移至生产环境？ |
| API 参考 | /docs/api_reference.md | 对外查询接口的鉴权方式、参数列表与返回格式是什么？ |
| 部署方案 | /docs/deployment.md | 支持哪些部署方式（Docker、Systemd、Supervisor），各有什么注意事项？ |
| 数据格式 | /docs/data_schema.md | 资源录入的 JSON 结构、必填字段与扩展字段如何定义？ |

## 资源列表

以下收录的全部外部链接均由社区成员提交并经初始可用性验证，用户可依据自身需要选择性接入。项目组不对第三方资源的可用性与内容准确性承担连带责任。

数据分类 - 综合信息接口

<code>https://yijiajishibifenc.org.cn</code>

<code>https://fajiajishibifenc.org.cn</code>

数据分类 - 实时赛事比分

<code>https://zuqiubisaijieguoc.org.cn</code>

<code>https://yingchaobifenc.org.cn</code>

数据分类 - 联赛积分与排名

<code>https://xijiabifenc.org.cn</code>

<code>https://dejiabifenc.org.cn</code>

<code>https://yijiabifenc.org.cn</code>

## 项目结构

```
linkhub/
├── app/                                 # 核心应用包
│   ├── __init__.py                      # 应用工厂函数，创建 Flask 实例
│   ├── routes/                          # 路由视图模块
│   │   ├── public.py                    # 公开页面（首页、分类浏览、详情）
│   │   └── admin.py                     # 管理后台（资源增删改、刷新触发）
│   ├── models/                          # 数据模型与 ORM 映射
│   │   ├── resource.py                  # Resource 实体，包含 URL、标签、状态字段
│   │   ├── check_log.py                 # 健康检查日志记录模型
│   │   └── user.py                      # 管理员账户模型
│   ├── services/                        # 业务逻辑服务层
│   │   ├── checker.py                   # 异步健康检查调度器实现
│   │   ├── stats.py                     # 响应时间统计与可用性计算
│   │   └── exporter.py                  # 收藏分组导出为 JSON/CSV 的工具函数
│   ├── templates/                       # Jinja2 模板文件
│   │   ├── base.html                    # 基础布局模板
│   │   ├── index.html                   # 首页资源列表与筛选面板
│   │   └── admin/                       # 后台管理页面模板集
│   └── static/                          # 静态资源（CSS、JavaScript、图标）
│       ├── css/                         # 基于 Bootstrap 5 的自定义样式表
│       └── js/                          # 前端交互脚本（筛选、收藏、图表渲染）
├── config/                              # 配置文件夹
│   ├── default.py                       # 默认配置（开发环境）
│   └── production.py                    # 生产环境覆盖配置（通过环境变量加载）
├── data/                                # 数据存储目录
│   ├── linkhub.db                       # SQLite 数据库文件（首次初始化生成）
│   └── seed/                            # 初始资源种子数据 JSON 文件
├── tests/                               # 单元测试与集成测试
│   ├── test_checker.py                  # 健康检查服务单元测试
│   └── test_api.py                      # API 接口返回状态与数据完整性测试
├── scripts/                             # 运维辅助脚本
│   ├── backup_db.sh                     # 数据库定时备份脚本
│   └── refresh_all.sh                   # 强制全量刷新资源状态脚本
├── requirements.txt                     # Python 依赖列表（固定版本）
├── manage.py                            # 命令行入口（开发服务器、数据库初始化）
├── README.md                            # 项目说明文档（本文件）
└── LICENSE                              # MIT 许可证文本
```

## 贡献指南

我们欢迎社区成员以多种方式参与项目改进，包括但不限于新增资源提交、现有链接可用性反馈、文档错误修正以及功能建议。

1. 在 GitHub 仓库页面点击 Fork 按钮创建个人复刻，将远端仓库克隆至本地开发环境。

2. 新建以 `feature/` 或 `fix/` 为前缀的分支，例如 `feature/add-football-resource`，避免直接在主分支上修改。

3. 若为新增外部资源，请按照 `data/seed/resource_schema.json` 中定义的格式填写完整字段，包括名称、URL、分类标签、数据粒度描述及建议刷新间隔。

4. 提交前执行本地测试套件 `python -m pytest tests/`，确保所有已有测试用例通过，并为新增功能补充对应的测试方法。

5. 推送分支至个人仓库，随后通过 GitHub 界面发起 Pull Request 至主仓库的 `main` 分支，在描述中清晰列出变更内容与自测结论。

## 常见问题

Q：健康检查探测失败时，系统会立即移除该资源吗？

A：不会。系统采用渐进式降级策略，单次探测失败仅记录日志并将该资源标记为「待观察」状态。连续三次探测失败后，资源状态变更为「异常」，前端页面默认隐藏异常资源但保留手动查看入口。连续七天处于异常状态的资源将由管理员人工审核后决定是否下架。

Q：如何将本地开发的 SQLite 数据库迁移至生产环境的 PostgreSQL 或 MySQL？

A：项目数据层使用 SQLAlchemy ORM，理论上支持多种关系型数据库后端。迁移时需在 `config/production.py` 中修改 `SQLALCHEMY_DATABASE_URI` 连接串，然后执行 `python manage.py db upgrade` 自动建表。现有 SQLite 数据可通过内置的 `python manage.py export_db --format json` 导出后再使用 `python manage.py import_db --source export.json` 导入新库，注意外键约束顺序。

Q：对外只读 API 接口的访问频率限制是多少？

A：未鉴权的匿名请求限制为每分钟 30 次，超出限制后返回 429 状态码并附带 `Retry-After` 头。通过管理后台注册的授权应用可使用 API Key 鉴权，限制放宽至每分钟 300 次。如需更高配额请联系项目维护团队进行商业授权协商。

## 许可证

本项目采用 MIT 许可证，允许任何个人或组织免费使用、复制、修改、合并、发布、分发、再许可及销售本软件的副本，但需在软件的全部副本或重要部分中包含原始版权声明与许可声明。详细信息请参阅项目根目录下的 LICENSE 文件。

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
