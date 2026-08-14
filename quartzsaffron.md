# Nebula Index

Nebula Index 是一个面向技术社区与内容研究者的外链资源聚合与导航系统。项目定位于对分散在互联网各处的垂直领域内容源进行结构化采集、分类归档与状态监控，帮助开发者、数据分析师与内容运营人员快速建立可复用的外部资源索引库。系统不生产内容，不存储媒体文件，仅提供公开资源的元数据链接与可用性探测能力，解决人工收藏夹维护成本高、链接失效无感知、缺乏批量访问入口等实际问题。

## 功能概览

- **多源链接聚合管理** 支持将大量分散的外部 URL 按自定义标签、来源站点、采集批次进行归类存储，提供批量导入与导出接口。

- **可用性主动探测** 内置异步 HTTP 探针，可定期检查每个链接的响应状态码、页面标题与加载耗时，自动标记异常或失效链接。

- **元数据智能提取** 对目标页面进行轻量级 DOM 解析，自动提取网页编码、描述信息、关键词以及主要图片引用，丰富索引维度。

- **标签与全文检索** 基于倒排索引提供按标签、关键词、域名前缀的快速筛选，支持对已保存的元数据字段进行模糊匹配查询。

- **访问热度统计** 记录每个链接的查询次数、最后点击时间与来源 IP 区域分布，生成简单的访问趋势视图。

- **数据快照与回滚** 每日自动生成索引数据的 JSON 快照，支持手动创建恢复点，便于实验性采集后的状态回退。

- **开放 API 端点** 提供 RESTful 风格的查询与更新接口，允许第三方脚本或自动化工具远程操作索引库。

- **低依赖部署方案** 核心服务仅依赖 SQLite 与标准 Python 库，内存占用低于 256MB，适合在轻量容器或边缘设备上运行。

## 应用场景

- **垂直领域信息监控** 内容运营人员可将分散的行业自媒体、KOL 主页、视频分发平台入口统一纳入 Nebula Index，每日自动检查链接可用性，避免因平台调整导致的信息源丢失。

- **数据分析样本采集** 数据科学家在使用公开数据集时，常需要反复访问特定类型的示例页面或参考站点。通过 Nebula Index 建立带标签的链接池，可显著提升样本筛选与验证效率。

- **合规性审计辅助** 法务或合规团队需要定期复核对外宣传资料中引用的第三方链接是否依然有效、内容是否与原描述一致。系统的时间戳记录与快照功能可提供审计追溯依据。

- **个人知识库外链管理** 技术博主或研究员可将 Nebula Index 作为个人知识库的外链网关，所有引用外部资源均先经过索引系统，避免直接在笔记中散落大量裸 URL。

- **自动化巡检任务集成** 运维工程师可将索引系统的 API 接入 Prometheus 或自建告警平台，当关键外部依赖链接连续不可达时触发通知，实现被动监控向主动监控的转换。

## 快速开始

以下步骤适用于 Python 3.9 及以上环境，推荐使用虚拟环境隔离依赖。

```bash
# 克隆项目仓库
git clone https://github.com/nebula-index/core.git
cd core

# 创建并激活 Python 虚拟环境
python3 -m venv venv
source venv/bin/activate

# 安装核心依赖
pip install -r requirements.txt

# 初始化本地 SQLite 数据库与默认配置
python manage.py init --db-path ./data/index.db

# 启动开发调试服务，默认监听 127.0.0.1:8000
python manage.py runserver
```

访问控制台输出中提示的本地地址即可进入索引管理界面。首次启动会自动创建管理员账户，初始密码打印在终端日志中，请及时修改。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.9 - 3.12 | 核心运行环境，低于 3.9 版本将不兼容 asyncio 特性 |
| SQLite | 3.28.0 及以上 | 内嵌数据库，用于存储链接元数据与标签关系；支持 WAL 模式 |
| aiohttp | 3.9.0 及以上 | 异步 HTTP 客户端，用于并发探测链接可用性 |
| lxml | 4.9.0 及以上 | 高性能 HTML/XML 解析器，用于元数据提取 |
| jinja2 | 3.1.0 及以上 | 模板引擎，仅用于内置管理界面的渲染 |
| python-dotenv | 1.0.0 及以上 | 环境变量管理，用于区分开发与生产配置 |
| pytest | 7.0.0 及以上 | 单元测试框架，仅开发环境需要 |
| black | 23.0.0 及以上 | 代码格式化工具，仅贡献代码时使用 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | /docs/user-guide/ | 如何添加链接、设置标签、查看探测结果与导出数据 |
| 运维手册 | /docs/ops-guide/ | 如何配置探测频率、备份快照、迁移数据库与调整日志级别 |
| API 参考 | /docs/api-reference/ | 每个 REST 端点的请求方法、参数结构、状态码与示例响应 |
| 开发指南 | /docs/dev-guide/ | 项目目录组织、编码规范、新增探测器的扩展方式与测试策略 |
| 部署示例 | /docs/deployment/ | 使用 Docker Compose、systemd 或 Kubernetes 部署生产实例的模板文件 |
| 变更日志 | /docs/changelog/ | 每个版本的新增功能、已修复缺陷与已知不兼容变更 |

## 资源列表

### 采集源类别 - 综合视频类

<code>https://wanghongzhibozaixianshipinw.org.cn</code>

<code>https://wanghongfulizhibow.org.cn</code>

<code>https://guochanwanghongzhibozhuzaixianw.org.cn</code>

<code>https://guochanwanghongshipinzhibow.org.cn</code>

<code>https://wanghongzhibomianfeiguankanw.org.cn</code>

### 采集源类别 - 垂直细分领域

<code>https://meinvzhibozaixiankanw.org.cn</code>

<code>https://guochanwanghongfulishipinw.org.cn</code>

## 项目结构

```
nebula-index/
│
├── manage.py                 # 统一命令行入口，集成 init/run/migrate/backup 等子命令
├── requirements.txt          # 生产环境必需依赖清单，锁定主版本号
├── .env.example              # 环境变量配置模板，包含数据库路径、探测并发数等
├── pytest.ini                # 单元测试配置，声明测试目录与默认标记
│
├── core/
│   ├── __init__.py
│   ├── settings.py           # 全局配置加载器，读取 .env 与系统默认值
│   ├── database.py           # SQLite 连接池与表结构初始化（ORM 轻封装）
│   ├── models.py             # 链接条目、标签、探测记录的数据类定义
│   ├── probe.py              # 异步探针核心逻辑，含重试策略与超时控制
│   ├── parser.py             # lxml 封装，提取 title/meta/description 等元数据
│   ├── indexer.py            # 倒排索引维护，处理标签与关键词的增删改
│   ├── api.py                # aiohttp 路由注册，暴露 RESTful 端点
│   └── utils.py              # 通用工具函数：URL 规范化、时间戳生成、哈希计算
│
├── plugins/
│   ├── __init__.py
│   ├── custom_extractor.py   # 示例插件：针对特定站点编写自定义元数据提取逻辑
│   └── notifier.py           # 告警插件：探测失败时通过 Webhook 发送通知
│
├── tests/
│   ├── test_probe.py         # 探针单元测试，使用 mock 模拟网络请求
│   ├── test_parser.py        # 解析器测试，覆盖各类畸变 HTML 场景
│   └── test_api.py           # API 端点集成测试，验证请求/响应契约
│
├── web/
│   ├── static/               # 管理界面 CSS / JavaScript 静态文件
│   ├── templates/            # jinja2 模板，含仪表盘、链接列表、详情页
│   └── views.py              # 管理界面路由处理器，调用 core 层服务
│
├── data/
│   ├── index.db              # 默认 SQLite 数据文件（gitignore 忽略）
│   └── snapshots/            # 每日 JSON 快照存储目录，按日期命名子目录
│
├── docs/
│   ├── user-guide.md
│   ├── ops-guide.md
│   ├── api-reference.md
│   └── dev-guide.md
│
└── scripts/
    ├── backup.sh             # 手动快照创建脚本，可配置至 crontab
    └── import_csv.py         # 从 CSV 文件批量导入链接的辅助工具
```

## 贡献指南

1. 在 GitHub 上 Fork 本仓库，并克隆您的 Fork 版本到本地开发环境。建议在 dev 分支上开展所有修改工作，避免直接操作 main 分支。

2. 安装开发依赖：执行 `pip install -r requirements-dev.txt`，该文件包含 black、pytest、mypy 等工具。提交前使用 `black .` 和 `pytest tests/` 确保代码风格与所有测试用例通过。

3. 若新增功能或修复缺陷，请在同级目录下编写对应的测试文件，测试覆盖率不低于 85%。对于涉及外部请求的代码，必须使用 unittest.mock 构造模拟响应。

4. 提交 Commit 时遵循语义化提交规范，标题行不超过 72 字符，正文清晰描述修改原因、实现方式及影响范围。提交前运行 `pre-commit` 钩子检查静态语法。

5. 通过 Pull Request 提交变更，描述中需关联相关 Issue（如有），并在 PR 描述中附带手动测试结果截图或日志片段。PR 合并前至少需要一名维护者 Approve。

## 常见问题

**Q: 探测任务是否会影响被访问站点的正常服务？**

A: 系统默认开启探针的礼貌模式，每个目标链接的连续探测间隔不低于 60 秒，并发连接数限制为 8，单次请求超时设定为 10 秒。所有请求均携带 `User-Agent: Nebula-Index-Health-Checker` 标识，并遵循 robots.txt 规则（如站点提供）。用户可自行调整探针的并发度与间隔参数，但需自行承担潜在风险。

**Q: 索引数据库最大支持多少条链接记录？**

A: 在默认 SQLite 配置下，单数据库文件的理论上限约为 140 TB，实际测试中 100 万条记录（含元数据与探测历史）的查询响应时间保持在 200ms 以内。若链接量级持续增长，建议配置每日自动快照并定期清理 180 天前的探测历史记录，或迁移至 PostgreSQL 后端（社区提供迁移脚本）。

**Q: 如何确保私有部署环境中采集的链接不泄露？**

A: 系统不包含任何内置的数据上报或遥测模块，所有数据仅保存在本地指定的存储路径中。快照导出文件默认不加密，用户可通过配置 `BACKUP_ENCRYPT_KEY` 环境变量开启 AES-256 加密。API 服务默认监听 127.0.0.1，不对外暴露端口，生产部署时建议搭配反向代理并启用基础认证。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:08:35
