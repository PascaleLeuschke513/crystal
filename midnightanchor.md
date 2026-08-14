# Resource Navigator

Resource Navigator 是一个面向开发者、技术研究人员与内容分析师的垂直领域资源导航与聚合系统。项目定位于对特定类型公开可用在线资源进行结构化整理、可用性监测与元数据提取，帮助用户在海量信息中快速定位符合需求的稳定数据源。

目标用户包括需要构建多语言语料库的自然语言处理工程师、进行区域数字内容生态研究的社会科学学者、以及需要持续监测特定类别公开数据可用性的运维与数据分析人员。系统通过自动化检测与人工校验结合的方式，维护一个高可用性的资源索引，并提供统一的访问入口与状态反馈机制。

## 功能概览

- **资源可用性实时监测**：系统后台定时对收录的每个资源执行HTTP请求状态检查，自动标记状态码异常或响应超时的条目，并提供最近7天的可用性趋势图表。

- **结构化元数据提取**：对可访问的资源页面自动提取标题、描述、语言标签、内容分类等关键元信息，并存入结构化检索数据库。

- **自定义分类与标签体系**：用户可根据研究或开发需要，为资源添加自定义分类标签、备注和重要性等级，构建个人化的资源视图。

- **多维度检索与过滤**：支持按域名关键词、状态码、内容语言、最后验证时间、响应时长等多个维度组合过滤，快速筛选目标资源。

- **历史状态快照对比**：记录每个资源在每次检测时的HTTP头信息与响应摘要，支持按时间轴对比状态变化，便于追溯服务中断或内容变更节点。

- **公开API接口**：提供基于RESTful风格的公共API，允许第三方程序以JSON格式获取资源列表、状态摘要及详细元数据，便于集成至自动化工作流。

## 应用场景

- **构建多语言字幕语料库**：NLP研究人员可利用本导航系统发现并持续监测提供多语言字幕文件的资源站点，批量获取用于机器翻译模型训练或语言对比分析的数据源。

- **区域数字内容生态监测**：社会科学研究者可通过系统定期记录特定区域或语言类别资源的在线状态与响应性能，分析基础设施稳定性与内容可及性变化趋势。

- **自动化数据管道资源预热**：数据工程团队在构建内容聚合或备份管道时，可将本系统作为资源发现与健康检查前置模块，仅对通过可用性验证的资源发起后续采集任务。

- **个人媒体库资源整理**：内容爱好者可利用分类与备注功能，对自己关注的音视频资源站点进行系统化归档和可用性追踪，避免遗忘或链接失效。

## 快速开始

以下步骤帮助您在本地环境快速部署 Resource Navigator 的核心监测模块。

```bash
# 1. 克隆项目仓库
git clone https://github.com/resource-navigator/core.git
cd resource-navigator

# 2. 安装Python依赖（建议使用虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 3. 初始化配置文件与本地数据库
cp config.example.yml config.yml
python scripts/init_db.py

# 4. 启动监测调度服务
python scheduler.py --interval 3600 --output reports/
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.9 及以上 | 核心运行环境，用于调度器、API与数据处理 |
| PostgreSQL | 13.0 及以上 | 生产环境推荐数据库，存储资源元数据与历史快照 |
| Redis | 6.2 及以上 | 可选组件，用于缓存API响应与分布式锁 |
| requests | 2.28.0 及以上 | HTTP请求库，用于执行资源可用性检测 |
| lxml | 4.9.0 及以上 | HTML/XML解析库，用于元数据提取 |
| pytest | 7.4.0 及以上 | 仅开发与测试环境需要 |

## 文档导航

| 层面 | 目录/文档 | 解答的问题 |
|------|----------|----------|
| 用户指南 | docs/user-guide/getting-started.md | 如何安装部署、配置监测参数与查看首个报告？ |
| 用户指南 | docs/user-guide/api-reference.md | 公开API的详细端点、参数格式与调用示例有哪些？ |
| 运维手册 | docs/ops/deployment-checklist.md | 生产环境部署所需的网络、数据库与进程守护配置清单？ |
| 运维手册 | docs/ops/troubleshooting.md | 常见检测失败原因（超时、403、DNS错误）的排查与处理方案？ |
| 开发者文档 | docs/dev/contribution-guide.md | 新增资源解析器或自定义检测逻辑的开发流程与代码规范？ |
| 开发者文档 | docs/dev/architecture-overview.md | 系统模块划分、数据流与核心类设计说明？ |

## 资源列表

本系统当前所收录与维护的公开资源导航条目，按类别分组陈列如下。注意：所有URL均严格保持用户提供的原始格式，系统不保证其可访问性，实际状态以监测报告为准。

通用在线资源类：

<code>https://mianfeiguankanzaixianguankanb.org.cn</code>

<code>https://jiujiushipinzaixianguankanb.org.cn</code>

<code>https://oumeizaixianguankanshipinb.org.cn</code>

<code>https://rihanshipinmianfeizaixianguankanb.org.cn</code>

<code>https://mianfeigaoqingshipinzaixianguankanb.org.cn</code>

字幕与语言资源类：

<code>https://renqixiliezhongwenzimuwb.org.cn</code>

<code>https://rihanmeinvzhongwenzimub.org.cn</code>

## 项目结构

```
resource-navigator/
├── config/                         # 配置文件夹
│   ├── default.yml                 # 默认全局配置（端口、检测超时、并发数）
│   └── custom/                     # 用户自定义配置存放目录
│       └── sources.example.json    # 初始资源列表模板
├── core/                           # 核心业务逻辑模块
│   ├── checker/                    # 可用性检测子模块
│   │   ├── http_client.py          # 封装requests，处理代理与重试策略
│   │   └── status_analyzer.py      # 状态码与响应头解析逻辑
│   ├── parser/                     # 元数据解析子模块
│   │   ├── html_extractor.py       # 基于XPath的标题/描述提取
│   │   └── language_detector.py    # 基于文本采样的语言识别
│   └── storage/                    # 数据持久化子模块
│       ├── db_client.py            # PostgreSQL异步连接池封装
│       └── schema/                 # 数据库迁移脚本与表定义
├── api/                            # RESTful API服务模块
│   ├── app.py                      # FastAPI应用入口
│   └── endpoints/                  # 按资源划分的路由处理函数
├── scripts/                        # 运维与辅助脚本
│   ├── init_db.py                  # 初始化数据库表与索引
│   └── import_sources.py           # 从CSV或JSON批量导入资源
├── tests/                          # 单元测试与集成测试
│   ├── test_checker.py             # 检测模块模拟测试
│   └── fixtures/                   # 测试用的模拟响应数据
├── reports/                        # 检测报告生成输出目录
│   └── 2026-08-14/                 # 按日期归档的JSON与HTML报告
├── scheduler.py                    # 周期性调度器主程序
├── requirements.txt                # Python第三方依赖清单
└── README.md                       # 项目概述与入门指引（本文件）
```

## 贡献指南

我们欢迎并感谢任何形式的贡献，包括但不限于新增资源解析器、优化检测性能、完善文档和报告错误。

1.  **阅读行为准则**：请首先查阅 `CODE_OF_CONDUCT.md` 文件，了解社区交流的基本准则与预期行为。
2.  **查找或创建议题**：在GitHub Issues页面查找您感兴趣的任务或尚未覆盖的资源类型。若提交新资源收录请求，请提供域名、预期分类和可访问性证据。
3.  **派生项目并本地开发**：派生项目至个人账户，克隆至本地，并依据上述“安装要求”搭建开发环境。请务必在功能分支上进行开发。
4.  **编写测试与代码**：新增或修改功能时，请在 `tests/` 目录下补充对应的单元测试，确保测试覆盖率达到要求。提交代码前运行全部测试套件。
5.  **提交拉取请求**：推送分支至派生仓库，向主仓库的 `main` 分支提交拉取请求。请清晰描述变更内容、关联议题编号以及测试结果摘要。

## 常见问题

**问：为什么系统检测到大量资源返回HTTP 403状态码？这会影响使用吗？**

答：HTTP 403通常表示目标服务器拒绝了我们的检测请求。这可能是由于服务器配置了反自动化访问策略、IP黑名单或需要特定的User-Agent头。系统会将这些资源标记为“不可用”，但不会自动剔除。您仍然可以在API查询中通过`include_unavailable=true`参数获取它们。对于持续403的资源，建议通过自定义检测配置更换请求头或代理进行复测。

**问：我可以添加自己关注的私有资源进行监测吗？配置会公开吗？**

答：可以。您可以通过编辑 `config/custom/sources.local.yml` 文件添加私有资源列表，该文件默认被 `.gitignore` 忽略，不会提交至公开仓库。系统在启动时会自动合并默认配置与自定义配置。请注意，本地监测报告仅存储在您的部署环境中，不会同步至任何公共服务器。

**问：如何调整检测频率和并发数，以避免对目标服务器造成压力？**

答：您可以在 `config/default.yml` 中修改 `checker.interval`（检测间隔，单位秒）和 `checker.concurrency`（同时检测的并发线程数）参数。对于资源较多的场景，建议将并发数设置为5-10，间隔不小于3600秒，以体现基本的网络礼仪。修改后重启调度器服务即可生效。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
