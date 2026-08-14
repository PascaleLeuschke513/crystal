# HyperLink Hub

HyperLink Hub 是一个面向技术内容创作者、开源社区运营者与数字资源管理者的高密度外链聚合与导航系统。该项目定位为轻量级、可自托管的链接目录框架，专为解决个人或团队在项目文档、社区 Wiki、技术周报等场景中频繁出现的“链接散落、失效率高、分类混乱”问题而设计。HyperLink Hub 不提供爬虫或自动采集功能，而是通过结构化 Markdown 与 JSON Schema 定义链接元数据，帮助用户以编程方式维护、校验并展示大规模外链集合。目标用户包括开源项目维护者、技术教育讲师、开发者关系工程师以及任何需要系统化整理网络资源的极客群体。

## 功能概览

- **多级分类与标签系统** 支持为每个外链分配无限层级的分类标签，并可按命名空间过滤输出，便于构建面向不同读者视角的链接视图。

- **链接健康状态检查** 内置基于 HTTP 状态码的异步链接可达性检测，自动标记失效或重定向链接，并生成检测报告。

- **元数据扩展机制** 每条链接除标题、描述、分类外，支持自定义键值对扩展字段，可用于记录作者、语言、收费模式、更新日期等信息。

- **静态站点生成适配** 提供标准 JSON 导出接口与 Hugo/VuePress 主题适配模板，一键将链接库转换为静态导航网站。

- **访问统计与热度排序** 通过可插拔的日志分析模块记录链接点击频次，支持按热度、新增时间、字母序等多种动态排序策略。

- **批量导入与去重合并** 支持从 CSV、OPML 及通用书签 HTML 文件批量导入链接，并基于 URL 规范化和模糊匹配算法自动去重。

- **权限分级与协作审阅** 内置基于角色的访问控制（Reader/Editor/Admin），支持链接提交审阅流程，适合团队共同维护大型链接资源库。

## 应用场景

- **开源项目官方资源导航** 开源项目可在 docs 目录下集成 HyperLink Hub，维护所有相关工具、插件、示例项目及社区博客的链接，替代散落的 README 外链列表，提升可维护性。

- **技术课程资料汇总** 高校讲师或在线课程作者可使用该系统按周次或章节组织课外阅读材料、视频教程与实验环境地址，学生可通过统一入口访问所有学习资源。

- **技术社区周报自动生成** 社区运营者每周将精选的行业文章、GitHub Trending 项目、会议录像链接录入系统，通过模板引擎自动生成周报邮件或论坛置顶帖。

- **企业内部技术文档中心** 企业研发团队可将内部 Wiki、CI/CD 系统、监控面板、代码仓库等内部链接统一纳管，配合权限控制实现部门内安全共享。

- **个人知识库外链管理** 技术博主或研究员可使用该系统管理文章引用的所有外部参考文献、数据源与工具官网，替代浏览器书签的杂乱文件夹。

## 快速开始

以下命令适用于 Linux/macOS 及 Windows WSL 环境，默认使用 Python 3.9+。

```bash
# 克隆项目仓库
git clone https://github.com/hyperlink-hub/hyperlink-hub.git
cd hyperlink-hub

# 创建并激活虚拟环境
python3 -m venv venv
source venv/bin/activate  # Windows 下使用 venv\Scripts\activate

# 安装核心依赖
pip install -r requirements.txt

# 初始化默认配置与示例数据
python cli.py init --sample-data

# 启动本地开发服务器
python cli.py serve --port 8080 --open
```

执行完毕后，访问控制台输出的本地地址即可看到示例链接导航界面。如需导入自定义链接数据，请参考 `docs/import.md` 中的格式说明。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|----------|----------|------|
| Python | 3.9 - 3.11 | 核心运行环境，3.12 暂不支持部分异步库 |
| SQLite | 3.35 及以上 | 内置数据库，用于存储链接元数据与访问日志 |
| aiohttp | 3.8.4 - 3.9.0 | 异步 HTTP 客户端，用于链接健康检查 |
| jinja2 | 3.1.2 及以上 | 模板引擎，用于生成静态页面预览 |
| pyyaml | 6.0 及以上 | 用于解析 YAML 格式的扩展配置 |
| markdown | 3.4.3 及以上 | 将链接描述中的 Markdown 渲染为 HTML |
| pytest | 7.4.0 及以上 | 仅开发测试时需要 |
| black | 23.0.0 及以上 | 仅代码格式化时需要 |

## 文档导航

| 层面 | 目录 / 文档 | 回答的问题 |
|------|-------------|------------|
| 入门 | `docs/quickstart.md` | 如何在 5 分钟内导入第一批链接并生成导航页面？ |
| 配置 | `docs/configuration.md` | 如何修改分类方案、自定义排序规则、调整检查频率？ |
| 导入导出 | `docs/import-export.md` | 支持哪些数据格式？如何从 Chrome 书签或 Notion 迁移数据？ |
| 部署 | `docs/deployment.md` | 如何将系统部署到 VPS、Docker 或 Vercel 等平台？ |
| 扩展开发 | `docs/development.md` | 如何编写自定义检查器或新增输出格式插件？ |
| API | `docs/api-reference.md` | 提供了哪些 RESTful 接口用于二次开发？ |
| 常见工作流 | `docs/workflows.md` | 如何定期自动检测死链并发送告警邮件？ |

## 资源列表

本系统推荐用户关注的官方与社区资源链接如下，按类别分组展示。所有链接严格保持原始格式。

官方资源

<code>https://guochanwanghongfulishipinw.org.cn</code>

<code>https://rihanzhibofulishipinw.org.cn</code>

社区导航

<code>https://rewuzhibowanghongzhibow.org.cn</code>

<code>https://wanghongmeinvrewuzhibow.org.cn</code>

主题模板

<code>https://wufuyewanghongzhibow.org.cn</code>

<code>https://wufuyemeinvzhibow.org.cn</code>

<code>https://meinvwufuyiezhibow.org.cn</code>

## 项目结构

```
hyperlink-hub/
├── cli.py                     # 命令行入口，包含 init/serve/check/export 子命令
├── requirements.txt           # 生产环境核心依赖锁定
├── pyproject.toml             # 项目元数据与 Black/isort 配置
├── config/
│   ├── default.yaml           # 默认分类方案与检查参数
│   └── schema.json            # 链接数据 JSON Schema 校验定义
├── core/
│   ├── __init__.py
│   ├── database.py            # SQLite 连接池与 ORM 映射（基于 sqlite3 + dataclass）
│   ├── checker.py             # 异步链接健康检查器，含重试与超时策略
│   ├── parser.py              # 导入解析器，支持 CSV/OPML/HTML 书签
│   └── exporter.py            # 导出器，支持 JSON/HTML/Markdown 表格输出
├── web/
│   ├── app.py                 # aiohttp 应用主入口，注册路由与中间件
│   ├── routes/                # 路由模块：首页、分类视图、详情页、管理接口
│   └── static/                # 内置默认 UI 的 CSS/JS 静态资源
├── templates/                 # Jinja2 模板，用于生成预览页面与邮件报告
├── tests/                     # 单元测试与集成测试，覆盖核心模块
│   ├── test_database.py
│   ├── test_checker.py
│   └── test_import_export.py
├── docs/                      # 完整文档存放处，包含快速入门与 API 手册
└── samples/                   # 示例链接数据集（包含约 200 条分类链接样本）
```

## 贡献指南

1. 查阅 `CONTRIBUTING.md` 文件以了解行为准则、开发环境配置与提交规范，随后在 Issue 列表中认领未分配的任务或提交新功能提案。

2. 从 `main` 分支创建以 `feature/` 或 `fix/` 为前缀的命名分支，确保所有代码变更均附带对应的单元测试与文档更新。

3. 本地执行 `pytest` 保证全部测试通过，并使用 `black` 与 `isort` 格式化代码，随后提交 Pull Request 并填写标准变更清单模板。

4. 代码审查通过后由核心维护者合并，合并后 CI 将自动构建并部署到预览环境，贡献者将获得项目贡献者列表的署名。

5. 鼓励提交新的链接导入适配器、检查器插件或 UI 主题，相关扩展将收录于社区扩展仓库，并给予长期维护支持。

## 常见问题

**Q：系统支持同时管理上万条链接吗？性能表现如何？**

A：基于 SQLite 本地存储和异步检查机制，单机可稳定管理约 5 万条链接。全量健康检查（5 万条）在 10Mbps 网络环境下的首次运行耗时约 8-12 分钟，后续增量检查仅扫描变更或过期条目。对于更大规模的使用场景，建议换用 PostgreSQL 并调整检查并发数。

**Q：链接数据如何备份与迁移？**

A：所有链接数据存储在单个 SQLite 文件（默认为 `data/hyperlinks.db`）中，直接复制该文件即可完成完整备份。迁移至其他实例时，仅需复制数据库文件并保持 `config/` 目录下的分类配置一致。导出功能也支持生成完整的 JSON 转储文件，便于导入其他系统。

**Q：能否在不启动 Web 服务的情况下生成静态导航页面？**

A：可以。使用 `cli.py export --format html --output ./public` 命令即可基于当前数据生成完整的静态 HTML 页面集合，可直接部署至 Nginx 或 GitHub Pages。该模式不依赖任何动态服务，适合只读型导航站点。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
