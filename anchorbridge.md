# CloudStream 外链资源聚合平台

CloudStream 是一个面向开发者、技术内容创作者及互联网研究人员的开源外链资源聚合与导航系统。本项目不直接托管或代理任何第三方内容，而是通过严谨的链接分类、状态监测与结构化展示，帮助用户从海量网络信息中快速定位特定类型的公开视频流媒体资源、直播平台入口及娱乐内容聚合站点。

本项目定位为技术研究辅助工具，适用于需要批量分析公开直播平台生态、采集流媒体样本或进行网络内容分类研究的场景。通过标准化的数据输出格式与清晰的资源分类，CloudStream 可显著降低信息筛选成本，提升资源调研效率。

## 功能概览

- **智能链接分类与标签化**：根据 URL 结构、域名特征及页面元数据，自动将外链归入直播、短视频、娱乐内容等预设分类，并生成可自定义的标签体系。

- **批量资源状态监测**：定期对收录的链接进行 HTTP 状态码检查，标记异常链接（如 403、404、超时），并提供最近三次检测结果的趋势记录。

- **结构化数据导出**：支持将资源列表导出为 JSON、CSV 及 Markdown 表格格式，便于整合至数据分析流水线或文档系统中。

- **自定义分类视图**：允许用户按项目需求创建多个分类视图，每个视图可独立包含不同的链接集合与排序规则，适用于多团队协作场景。

- **链接变更追踪**：记录每个链接的标题、响应头关键字段（如 Content-Type、Server）的变更历史，便于追踪资源演进过程。

- **简易部署与集成**：提供基于 Python Flask 的轻量级 Web 仪表盘，以及可在 CI/CD 流程中使用的命令行接口（CLI）工具。

- **外链元数据增强**：通过可插拔的解析器，从链接对应的页面中提取描述信息、关键词及公共媒体元数据，丰富资源上下文。

## 应用场景

1. **流媒体生态研究**：研究人员可利用本平台批量采集公开直播站点入口，分析不同平台的内容类型分布、技术栈特征及可用性变化趋势。

2. **内容审核辅助**：内容审核团队可将本平台作为可疑链接的初步筛选入口，通过状态监测与分类视图快速定位高风险或异常资源。

3. **技术教学示例**：在网络编程或爬虫技术课程中，讲师可使用本项目的结构化链接数据作为教学示例，演示 HTTP 请求处理、数据解析及状态码分析。

4. **个人知识库构建**：技术博主或开发者可将频繁访问的流媒体资源站点通过本平台统一管理，并利用导出功能生成个人导航页。

## 快速开始

以下步骤将在本地环境中完成 CloudStream 的克隆、安装与初次运行。

```bash
# 1. 克隆项目仓库
git clone https://github.com/cloudstream-community/cloudstream-aggregator.git
cd cloudstream-aggregator

# 2. 创建并激活 Python 虚拟环境（推荐）
python3 -m venv venv
source venv/bin/activate  # Windows 系统请使用 venv\Scripts\activate

# 3. 安装核心依赖与 CLI 工具
pip install -r requirements.txt
pip install -e .

# 4. 初始化示例资源数据库（包含本批次预置链接）
python cli.py init-db --sample

# 5. 启动 Web 仪表盘（默认监听 5000 端口）
python cli.py run-server
```

启动后，在浏览器中访问 `http://127.0.0.1:5000` 即可查看资源列表与分类视图。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.8 - 3.11 | 核心运行环境，建议使用 3.10 稳定版 |
| Flask | 2.2.x | Web 仪表盘框架，用于提供可视化界面 |
| requests | 2.28.x | 用于执行 HTTP 状态检测与元数据抓取 |
| SQLite | 3.35+ | 内置轻量级数据库，用于存储资源记录与状态历史 |
| Git | 2.30+ | 用于克隆仓库及版本管理 |
| pip | 22.0+ | Python 包管理工具，用于安装依赖 |

## 文档导航

| 层面 | 目录/章节 | 回答的问题 |
|------|----------|----------|
| 入门指南 | docs/quickstart.md | 如何最快部署并看到资源列表？如何理解核心界面？ |
| 运维手册 | docs/administration.md | 如何配置自动状态监测？如何备份数据库？如何处理异常链接？ |
| 开发参考 | docs/development/api.md | 如何扩展自定义分类器？如何编写新的元数据解析插件？ |
| 数据字典 | docs/data-schema.md | 数据库表结构如何定义？各字段含义及取值范围是什么？ |

## 资源列表

本批次收录的链接属于“公开流媒体入口”分类，具体资源如下：

**直播平台入口**
- <code>https://shuaigefujifulizhibo.org.cn</code>
- <code>https://oubazhibomianfeiguankan.org.cn</code>
- <code>https://wanghongzhibofulizaixian.org.cn</code>

**视频与主播内容**
- <code>https://nvzhubozshipinzaixianguankan.org.cn</code>
- <code>https://xingganmeinvzhibotiaowu.org.cn</code>
- <code>https://hanguomeinvzhuborewu.org.cn</code>
- <code>https://zaixianbofangzhubo.org.cn</code>

## 项目结构

```
cloudstream-aggregator/
├── cli.py                      # CLI 入口，包含 init-db、run-server 等命令
├── requirements.txt            # 生产环境依赖列表
├── setup.py                    # 项目安装脚本
├── README.md                   # 项目说明文档（当前文件）
├── .env.example                # 环境变量配置模板（含监测间隔、端口等）
├── src/
│   ├── __init__.py             # 包初始化
│   ├── app.py                  # Flask 应用工厂与路由定义
│   ├── models.py               # SQLAlchemy ORM 模型定义（Resource, CheckHistory）
│   ├── schemas.py              # Pydantic 或 Marshmallow 序列化结构
│   ├── classifier.py           # 链接分类逻辑（基于规则与正则）
│   ├── fetcher.py              # HTTP 请求封装，含重试与超时控制
│   └── utils.py                # 通用辅助函数（日期处理、URL 规范化）
├── src/parsers/                # 可插拔的页面元数据解析器
│   ├── base.py                 # 解析器基类
│   ├── html_meta.py            # 从 HTML meta 标签提取信息
│   └── placeholder.py          # 占位解析器（返回空数据）
├── tests/                      # 单元测试与集成测试
│   ├── test_classifier.py
│   ├── test_fetcher.py
│   └── conftest.py             # pytest 固定装置
├── web/                        # Web 仪表盘静态资源
│   ├── static/
│   │   ├── style.css           # 基础样式表
│   │   └── dashboard.js        # 前端交互逻辑（状态筛选、自动刷新）
│   └── templates/
│       ├── base.html           # 基础模板
│       ├── index.html          # 资源列表主页
│       └── detail.html         # 单个资源详情与历史记录
├── data/                       # 数据存储目录
│   └── cloudstream.db          # SQLite 数据库文件（首次启动自动生成）
└── docs/                       # 扩展文档
    ├── quickstart.md
    ├── administration.md
    ├── development/
    └── data-schema.md
```

## 贡献指南

我们欢迎并感谢任何形式的贡献，包括但不限于代码、文档、测试用例及问题反馈。请遵循以下步骤：

1. **查阅现有议题**：在提交新议题前，请先浏览 GitHub Issues 列表，确认无人正在处理同类问题。如无匹配，请创建新议题并清晰描述建议或缺陷。

2. **派生项目并创建特性分支**：从主仓库派生（Fork）项目，并基于 `main` 分支创建您的特性分支（例如 `feature/add-rtmp-parser`）。请确保分支名称具有描述性。

3. **遵循编码规范**：Python 代码需符合 PEP 8 风格，并使用 `black` 和 `isort` 进行格式化。所有新增功能必须包含对应的单元测试，且测试覆盖率不应低于 80%。

4. **提交变更并签署开发者原产地证书（DCO）**：提交信息应简洁明了，说明变更动机与实现方式。所有提交必须包含 `Signed-off-by` 行，以证明您有权贡献该代码。

5. **发起拉取请求（PR）**：向主仓库的 `main` 分支发起 PR。在描述中引用相关议题编号，并确保 CI 流水线（含测试与静态检查）全部通过。PR 至少需要一名维护者审阅后方可合并。

## 常见问题

**Q：状态检测返回 403 状态码，是否意味着链接无效？**

A：不一定。HTTP 403 表示服务器理解请求但拒绝授权，这可能是由于服务器配置了反爬策略、需要特定请求头或 IP 被临时限制。本平台会如实记录 403 状态，但不会自动将此类链接标记为“失效”。建议用户结合链接的可用性历史与自身访问环境综合判断。对于持续返回 403 的链接，可考虑通过配置自定义请求头（如 User-Agent、Referer）进行重测。

**Q：如何自定义链接分类规则？**

A：您可以在 `src/classifier.py` 中修改 `CLASSIFICATION_RULES` 字典。该字典以分类名为键，值为一个包含 `patterns`（正则列表）和 `priority`（优先级整数）的字典。调整规则后，运行 `python cli.py reclassify` 命令即可对所有现有链接重新应用新分类。建议在修改前备份数据库文件。

**Q：平台是否支持添加非 HTTP 协议的资源（如 RTMP、WebSocket）？**

A：当前版本主要针对 HTTP/HTTPS 协议设计，状态检测模块仅支持 HTTP 方法。但对于 RTMP 等协议，您仍可将其 URL 作为文本记录录入，系统会保存其元数据，但不会执行状态检测。我们计划在后续版本中提供可扩展的协议检测适配器，欢迎贡献相关实现。

## 许可证

本项目采用 MIT 许可证进行开源。您可以自由使用、修改、分发本软件，包括用于商业目的，但必须保留原始版权声明和许可声明。详见项目根目录下的 `LICENSE` 文件。

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
