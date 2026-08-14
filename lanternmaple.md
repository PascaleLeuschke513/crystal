# Resource Navigator

Resource Navigator is a lightweight, high-performance technical resource aggregation and navigation system designed for developers, researchers, and technical content curators who need to organize, categorize, and rapidly access distributed online resources. The project addresses the common challenge of managing disparate, unstructured external links by providing a centralized, maintainable, and semantically indexed directory layer that sits on top of existing content providers. It is not a search engine nor a web scraper; it is a curated knowledge orchestration tool that enforces rigorous link governance, availability probing, and categorical partitioning to support reproducible research workflows and structured information architecture.

The primary target audience includes open-source documentation maintainers, data science teams managing external dataset references, DevOps engineers maintaining operational dashboards, and technical bloggers who require a reliable, version-controlled method to reference third-party streaming and media resources. Resource Navigator operates on the principle of "link as code," treating every external URL as a first-class entity with metadata, health status, and contextual tags. The system includes built-in link rot detection, response time monitoring, and automatic category inference based on domain patterns and content-type headers, thereby reducing the manual overhead of maintaining large resource indexes. By decoupling resource references from application logic, Resource Navigator enables clean separation of concerns, improves portability, and facilitates collaborative curation through standard pull-request workflows.

## 功能概览

- **Categorized Link Indexing** – Organizes external URLs into user-definable taxonomies with support for multi-tagging, hierarchical categories, and semantic grouping based on domain naming conventions.

- **Automated Health Probing** – Periodically checks each resource endpoint for HTTP status codes, TLS certificate validity, and response latency, flagging degraded or offline resources with visual indicators.

- **Markdown-Based Configuration** – All resource lists and category definitions are stored in plain Markdown files, enabling version control, diff-based code reviews, and seamless integration with existing documentation pipelines.

- **Static Site Generation** – Produces fully static HTML navigation pages from source Markdown and YAML metadata, requiring no server-side runtime or database dependencies for production deployment.

- **Resource Metadata Enrichment** – Supports optional annotations including description, maintainer contact, update frequency, content-type hints, and geographic origin for each entry.

- **Search and Filter Interface** – Provides client-side full-text search and category-based filtering powered by lightweight index files, enabling fast resource discovery without external API calls.

- **Link Integrity Reporting** – Generates periodic reports summarizing broken links, newly added resources, and category coverage statistics, suitable for email or logging integration.

- **Extensible Plugin System** – Allows custom pre-processing hooks for domain-specific validation, custom header injection, or integration with external monitoring services.

## 应用场景

- **Technical Documentation Portals** – Project maintainers can embed Resource Navigator as a dedicated "External References" section within their documentation sites, ensuring all cited third-party streaming platforms and media resources are centrally listed, version-pinned, and regularly verified for availability. This reduces documentation drift and improves reader trust.

- **Data Journalism and Research Repositories** – Research teams publishing datasets or investigative reports can use the system to catalog original video sources, live stream archives, and multimedia evidence links. The automated health checks provide transparency regarding resource accessibility at the time of publication, which is critical for reproducible fact-checking workflows.

- **DevOps Monitoring Dashboards** – Site reliability engineers can integrate Resource Navigator into internal observability portals to track the operational status of external media delivery endpoints, CDN origins, and partner streaming services. The probing subsystem provides early warning of outages affecting dependent systems.

- **Content Aggregation Newsletters** – Curators managing weekly technical digests can maintain a curated master list of recurring reference streams and video archives. The categorization and search features allow rapid selection of relevant resources for each themed edition without re-curating from scratch.

- **Educational Course Resource Pages** – Instructors teaching multimedia technology, network programming, or digital media production can publish a stable, versioned resource index for course participants. The static generation ensures the resource list remains accessible even after the course concludes, preserving links for future cohorts.

## 快速开始

The following commands clone the repository, install dependencies, and launch the development server with a sample resource catalog.

```bash
git clone https://github.com/resource-navigator/core.git
cd resource-navigator
npm install
npm run build
npm start
```

After execution, the navigation interface will be available at `http://localhost:8080`. The default configuration loads a sample catalog located in `config/sample-catalog.yaml`. To use your own resource list, replace `config/resources.md` with your curated entries following the established Markdown table format, then rebuild the static output using `npm run build:static`.

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Node.js | 18.x LTS 或更高 | 运行时环境，用于执行构建脚本和开发服务器 |
| npm | 9.x 或更高 | 包管理器，用于安装所有声明的依赖项 |
| Git | 2.30.x 或更高 | 版本控制系统，用于克隆仓库和管理配置变更 |
| libcurl | 7.68.x 或更高 | 系统级 HTTP 客户端库，用于健康探测子系统的底层连接 |
| yq | 4.x 或更高 | YAML 处理工具，用于解析和转换配置中的元数据字段 |
| markdownlint-cli | 0.33.x 或更高 | 代码质量检查工具，用于验证资源列表文件的格式合规性 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | `docs/getting-started.md` | 如何配置第一个资源目录？如何添加自定义类别？开发服务器如何启动？ |
| 配置参考 | `docs/configuration.md` | 支持哪些配置字段？YAML 结构与 Markdown 表如何对应？如何启用自动探测？ |
| 插件开发 | `docs/plugin-development.md` | 如何编写自定义验证器？如何添加新的输出格式？探测钩子的生命周期是什么？ |
| 运维手册 | `docs/operations.md` | 如何部署到生产环境？如何设置定期探测任务？如何解读健康报告？ |
| API 参考 | `docs/api-reference.md` | 提供了哪些编程接口？如何通过 JSON 端点获取资源状态？如何集成外部工具？ |

## 资源列表

### 直播与多媒体资源类别

- <code>https://wanghongmeinvrewuzhibo.org.cn</code>
- <code>https://wufuyewanghongzhibo.org.cn</code>
- <code>https://wufuyemeinvzhibo.org.cn</code>
- <code>https://meinvwufuyiezhibo.org.cn</code>
- <code>https://shuaigefujifulizhibo.org.cn</code>
- <code>https://oubazhibomianfeiguankan.org.cn</code>
- <code>https://wanghongzhibofulizaixian.org.cn</code>

## 项目结构

```
resource-navigator/
├── src/                             # 核心源代码目录
│   ├── core/                        # 基础引擎模块
│   │   ├── indexer.js               # 资源索引构建器，解析 Markdown 并生成内部映射
│   │   ├── probe.js                 # HTTP 健康探测器，管理超时、重试和状态缓存
│   │   └── category.js              # 分类推理引擎，基于域名词典和 TLD 规则
│   ├── generators/                  # 输出生成器
│   │   ├── static.js                # 静态 HTML 站点生成器，使用 Handlebars 模板
│   │   ├── json.js                  # JSON API 端点生成器，用于外部程序消费
│   │   └── report.js                # 健康报告生成器，输出 Markdown 和 CSV 格式
│   ├── plugins/                     # 内置插件集合
│   │   ├── tls-validator.js         # TLS 证书有效期和签发链验证插件
│   │   ├── latency-histogram.js     # 响应时间分位数统计插件
│   │   └── domain-whitelist.js      # 域名白名单过滤插件，用于企业合规场景
│   └── cli/                         # 命令行入口
│       ├── build.js                 # 构建命令实现
│       ├── serve.js                 # 开发服务器命令实现
│       └── probe.js                 # 手动触发探测命令实现
├── config/                          # 配置与资源定义
│   ├── resources.md                 # 主资源列表文件（Markdown 表格格式）
│   ├── categories.yaml              # 类别定义与别名映射
│   └── probe-policy.yaml            # 探测策略（间隔、超时、重试、告警阈值）
├── docs/                            # 用户文档（见上方导航表）
├── test/                            # 单元测试与集成测试
│   ├── unit/                        # 各模块单元测试用例
│   └── fixtures/                    # 测试用的样例资源列表和模拟响应数据
├── templates/                       # 静态站点 Handlebars 模板文件
├── output/                          # 构建输出目录（生成产物存放处，默认 .gitignore）
├── package.json                     # npm 项目清单
├── .markdownlint.json               # Markdown 语法检查规则配置
└── README.md                        # 本文件
```

## 贡献指南

1. 阅读 `docs/configuration.md` 和 `docs/plugin-development.md` 以了解内部数据模型和扩展机制。所有新增资源条目必须遵循 `resources.md` 中的表格格式，包含完整的 URL、类别标签和可选描述字段。

2. 从 `main` 分支创建功能分支，命名格式为 `feature/your-feature-name` 或 `fix/issue-number`。在提交前运行 `npm run lint` 和 `npm test` 确保代码风格一致且所有测试用例通过。新增功能必须附带对应的单元测试。

3. 提交变更时使用语义化提交信息（Conventional Commits），例如 `feat: add domain-whitelist plugin` 或 `fix: correct probe timeout handling on slow networks`。每个提交应保持逻辑原子性。

4. 发起 Pull Request 至 `main` 分支，并在描述中清晰说明变更动机、影响范围以及手动测试步骤。PR 需要至少一名核心维护者的代码审查批准。审查过程中若有修改意见，请在原分支上追加修正提交，不要 rebase 已审查的提交。

5. 合并后，CI 流水线将自动构建静态站点并部署到预发布环境。若涉及资源列表的批量更新，请同时更新 `docs/operations.md` 中对应的探测策略说明，确保运维团队知晓变更后的检查频率和告警条件。

## 常见问题

**问：资源列表中的 URL 不可用时会发生什么？探测机制是否会访问每个链接的内容？**

答：探测子系统仅执行轻量级 HEAD 请求并检查 HTTP 状态码、TLS 握手成功性和响应头中的 `Content-Type` 字段。它不会下载完整响应体，也不会执行任何 JavaScript 或解析页面内容。对于状态码在 400 及以上或连接超时的资源，系统会在 `output/reports/unhealthy.md` 中标记并记录失败原因。探测结果不影响静态站点的生成，但前端界面会显示状态颜色指示符（绿色可用、黄色降级、红色不可用）。用户可手动运行 `npm run probe -- --force` 立即触发全量重新探测。

**问：如何导入现有的书签或收藏夹数据？是否支持浏览器书签 HTML 格式？**

答：项目原生不提供直接的书签 HTML 解析器，但提供了转换辅助脚本 `tools/import-bookmark.js`，该脚本接受 Netscape 格式的书签导出文件，提取 URL、标题和文件夹层级，并生成符合 `resources.md` 表格结构的 Markdown 输出。转换后建议手动检查类别映射，因为自动分类基于域名字典可能不够精确。对于大规模迁移，我们推荐编写自定义插件来实现特定来源的映射规则。

**问：部署到生产环境时需要数据库吗？静态生成的内容如何更新？**

答：Resource Navigator 完全不需要数据库。所有资源数据和配置都存储在文本文件中，构建过程生成纯静态 HTML、CSS、JavaScript 和 JSON 索引文件。生产部署可以使用任何支持静态托管的服务（如 Nginx、S3 对象存储、GitHub Pages 等）。内容更新通过修改源文件并重新运行构建命令来实现。我们建议使用 CI/CD 流水线：当 `main` 分支上的 `config/resources.md` 或 `categories.yaml` 发生变更时，自动触发构建并将输出同步到生产环境。这种方式确保了每次更新都是原子性的、可回滚的，并且完整的变更历史保留在 Git 中。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
