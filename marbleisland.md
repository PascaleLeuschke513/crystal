# Nexus Archive

Nexus Archive 是一个面向开发人员、技术研究者和内容策展人的轻量级外链资源聚合与导航系统。该项目定位于解决个人或团队在浏览、收藏、分类及分享大量分散的网络资源（如技术文档、工具站点、数据源、媒体库等）时面临的效率低下与链接失效问题。它不提供资源存储或代理服务，而是通过结构化的元数据管理和简洁的索引页面，帮助用户构建可维护、可扩展的私有或公开网络资源目录。

目标用户包括需要系统化管理学习资料的技术爱好者、维护团队内部知识库的工程师，以及希望以极简方式分享优质外链集合的内容创作者。Nexus Archive 核心价值在于将无序的链接列表转化为带有分类、标签和状态监控的活文档，降低信息过载带来的认知负担。

## 功能概览

- **链接分类与标签管理**：支持为每个外链资源分配多个自定义标签和分类，便于按主题、领域或使用场景进行多维度筛选。
- **资源状态健康检查**：内置定时任务，可对已收录的 URL 进行可达性探测，自动标记异常链接（如返回 403、404 或超时），并提供状态报告。
- **Markdown 原生编辑**：所有资源数据以 Markdown 文件形式存储，支持通过文本编辑器或版本控制系统（Git）进行批量修改与历史追溯。
- **静态站点生成**：提供命令行工具，可将资源数据渲染为静态 HTML 页面，无需数据库支持，便于部署到任何 Web 服务器或托管平台。
- **资源导入与导出**：支持从 CSV、JSON 或浏览器书签文件（HTML）批量导入链接，并可导出为结构化数据格式用于迁移或备份。
- **自定义视图与筛选**：提供按域名、状态码、添加时间或自定义字段排序的列表视图，支持组合条件查询，快速定位特定资源。
- **链接变更追踪**：记录资源标题或 URL 的修改历史，便于团队协作时追溯变动原因。

## 应用场景

1.  **个人技术知识库构建**：开发者可将日常阅读的博客、API 文档、在线工具和学术论文链接统一收录，按编程语言、框架或领域分类，配合健康检查功能定期清理失效书签。
2.  **团队内部资源目录**：研发团队可使用 Nexus Archive 维护项目依赖的公共库镜像站、内部文档入口、CI/CD 工具地址等，通过静态站点在内部网络分享，确保成员快速找到权威资源。
3.  **开源项目外部链接汇总**：开源项目维护者可在 README 中引用由 Nexus Archive 生成的资源列表页，集中展示社区推荐的插件、教程或相关项目，方便贡献者与用户查阅。
4.  **内容策展与主题研究**：研究人员或博主可围绕特定技术主题（如机器学习、云原生）收集并注释相关资源，生成带分类注释的公开链接合集，辅助专题内容创作。
5.  **网站迁移与链接审计**：在网站改版或域名迁移时，利用系统导出的完整链接清单进行批量检查，识别需要更新或重定向的外部引用。

## 快速开始

以下步骤将指导您在本地环境快速启动 Nexus Archive 服务。

```bash
# 1. 克隆代码仓库
git clone https://github.com/nexus-archive/nexus-archive.git
cd nexus-archive

# 2. 安装项目依赖（使用 pip 和虚拟环境）
python3 -m venv venv
source venv/bin/activate  # Windows 系统请使用 venv\Scripts\activate
pip install -r requirements.txt

# 3. 初始化资源数据库（基于示例数据）
python manage.py initdb --sample-data

# 4. 启动本地开发服务器
python manage.py runserver --port 8080
```
启动后，打开浏览器访问 `http://localhost:8080` 即可查看资源导航界面。使用 `python manage.py --help` 可查看所有管理命令。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
| :--- | :--- | :--- |
| Python | 3.8 或更高版本 | 核心运行环境，用于执行管理脚本和本地服务器 |
| pip | 20.0 以上 | Python 包管理工具，用于安装依赖库 |
| Git | 任意近期版本 | 用于克隆仓库及版本控制（可选，但推荐） |
| 操作系统 | Linux / macOS / Windows | 支持主流操作系统，路径处理已做兼容 |
| 内存 | 最低 512 MB | 静态站点生成和本地服务运行所需内存 |
| 磁盘空间 | 至少 100 MB | 用于存放代码、依赖库及生成的资源索引文件 |
| 网络 | 出站连接 | 用于执行资源健康检查（若启用）及访问外链本身 |
| 浏览器 | 现代浏览器（Chrome/Firefox/Edge） | 用于查看生成的静态页面或管理界面 |

## 文档导航

| 层面 | 目录/章节 | 回答的问题 |
| :--- | :--- | :--- |
| 用户指南 | `docs/user-guide/` | 如何添加、编辑、分类和导入外部链接？健康检查如何配置与解读？ |
| 开发参考 | `docs/developer-guide/` | 项目架构是怎样的？如何扩展自定义字段或编写新的导入导出插件？ |
| 运维手册 | `docs/operations/` | 如何将系统部署到生产环境？如何配置定时任务进行链接监控？ |
| 设计文档 | `docs/design/` | 数据模型为何采用文件而非数据库？静态生成机制的性能考量是什么？ |

## 资源列表

本项目文档和示例中引用或推荐了以下外部资源链接。它们被收录于此以供参考，且均按用户原始数据原样列出。

**影视与媒体资源类**
- <code>https://zhongwenzimuzaixianyingshiyuanb.org.cn</code>
- <code>https://mianfeiguankanzaixianguankanb.org.cn</code>
- <code>https://jiujiushipinzaixianguankanb.org.cn</code>
- <code>https://oumeizaixianguankanshipinb.org.cn</code>
- <code>https://rihanshipinmianfeizaixianguankanb.org.cn</code>
- <code>https://mianfeigaoqingshipinzaixianguankanb.org.cn</code>
- <code>https://renqixiliezhongwenzimuwb.org.cn</code>

请注意，这些链接指向的外部站点与 Nexus Archive 项目无附属关系。资源健康检查功能可能报告其中部分链接返回异常状态（如 HTTP 403），此为正常现象，表示目标服务器拒绝了访问请求，而非本系统问题。您可根据需要将其从监控列表中排除或替换为有效地址。

## 项目结构

```
nexus-archive/
├── archive/                        # 核心应用目录
│   ├── core/                       # 核心逻辑模块
│   │   ├── checker.py              # 链接健康检查引擎（支持并发探测）
│   │   ├── parser.py               # 资源元数据解析器（Markdown/JSON）
│   │   └── renderer.py             # 静态页面渲染器（Jinja2 模板）
│   ├── models/                     # 数据模型定义
│   │   ├── resource.py             # 资源实体类（含标签、状态、时间戳）
│   │   └── category.py             # 分类树节点定义
│   ├── storage/                    # 存储适配器
│   │   ├── file_backend.py         # 文件系统读写实现
│   │   └── git_backend.py          # Git 版本控制集成（可选）
│   ├── cli/                        # 命令行接口子命令
│   │   ├── commands.py             # 子命令路由与参数解析
│   │   └── import_export.py        # 批量导入导出处理
│   └── web/                        # 内置 Web 服务（开发调试用）
│       ├── app.py                  # Flask 应用工厂
│       └── templates/              # 默认 UI 模板文件
├── data/                           # 用户数据存储目录（示例/生产）
│   ├── resources/                  # 每个资源一个 .md 文件
│   ├── categories.yaml             # 分类体系定义
│   └── config.yaml                 # 用户自定义配置（监控周期等）
├── tests/                          # 单元测试与集成测试
│   ├── test_checker.py
│   └── test_parser.py
├── docs/                           # 详细文档源码
├── requirements.txt                # 生产依赖列表
├── requirements-dev.txt            # 开发额外依赖
├── setup.py                        # 安装脚本入口
└── README.md                       # 项目概览（本文件）
```

## 贡献指南

我们欢迎并感谢任何形式的贡献。请遵循以下步骤参与项目：

1.  **报告问题或提议功能**：在 GitHub Issues 页面搜索是否已有类似议题。若无，请创建新议题，清晰描述您遇到的问题或期望的功能，并附上重现步骤或用例。
2.  **Fork 并克隆仓库**：将本项目 Fork 到您的账户下，然后克隆到本地进行开发。建议在开发前创建新的功能分支（如 `feature/your-feature-name`）。
3.  **编写或修改代码**：请遵循项目现有的代码风格（PEP 8），并为新增或修改的代码编写相应的单元测试，确保测试通过（运行 `pytest tests/`）。
4.  **提交变更并创建 Pull Request**：提交前请合并上游主分支的最新代码，解决可能存在的冲突。PR 描述请引用相关的 Issue 编号，并简要说明修改内容与影响范围。
5.  **参与代码审查**：提交 PR 后，维护者将进行审查。请根据反馈进行必要的调整。所有 PR 需通过持续集成（CI）检查后方可合并。

## 常见问题

**Q: 健康检查功能为什么会对某些链接返回 403 状态？如何解决？**
A: HTTP 403 状态表示目标服务器理解请求但拒绝提供内容。这通常是因为目标网站配置了反爬虫机制、防火墙规则，或检查了特定的 User-Agent 请求头。Nexus Archive 的检查器默认使用简单的 HTTP HEAD 请求，可能被目标服务器拦截。您可以在 `data/config.yaml` 中调整检查请求的 User-Agent 字段，或增加请求延迟时间。若仍无效，可在资源条目中手动将健康检查状态设置为“忽略”。

**Q: 项目必须部署为 Web 服务吗？能否只生成静态页面？**
A: 不需要。Nexus Archive 的核心功能之一是静态站点生成。您可以在命令行中执行 `python manage.py build --output-dir ./public`，系统将读取所有资源数据并渲染为纯 HTML、CSS 和 JavaScript 文件。您可以将生成的 `public` 目录内容部署到任何支持静态文件的托管服务（如 GitHub Pages、Netlify 或 Nginx）。

**Q: 如何批量更新资源链接的域名（例如从旧域名迁移到新域名）？**
A: 由于所有资源数据存储为独立的 Markdown 文件，您可以使用文本编辑器的“在文件中查找并替换”功能，或使用 `sed`、`awk` 等命令行工具对 `data/resources/` 目录下的文件进行批量字符串替换。操作前请务必备份数据目录。若需更复杂的转换逻辑，可编写自定义脚本调用 `core.parser` 和 `storage.file_backend` 模块实现。

## 许可证

本项目采用 MIT 许可证。您可以在遵守许可证条款的前提下自由使用、修改、分发本软件，包括用于商业目的。详细的许可证文本请参阅项目根目录下的 `LICENSE` 文件。

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
