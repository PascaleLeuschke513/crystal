# NexusIndex

NexusIndex 是一个面向技术内容策展、网络资源归档与交叉引用检索的静态索引系统。项目定位为可自我维护的外部资源映射与结构化导航枢纽，服务于需要频繁查阅、分类管理大量外部链接的技术作者、文档维护者及社区运营人员。NexusIndex 不产生原始内容，仅提供描述、标签、状态标记与关系映射，从而解决多源信息散落、链接失效与上下文缺失导致的检索效率问题。

## 功能概览

- **资源映射记录** 每条资源条目支持标题、描述、标签、状态码与最后检查时间的完整元数据记录。
- **多级标签体系** 支持创建层级标签树，便于按领域、用途、语言或合规状态进行任意维度筛选。
- **链接可用性监控** 内置基于 HTTP 状态码的被动检测逻辑，可在页面渲染时标记失效或重定向链接。
- **静态站点生成** 基于 Markdown 条目库生成完整静态 HTML 站点，无需数据库，适宜直接托管于各类静态托管服务。
- **导入与导出机制** 支持 JSON 与 CSV 格式的批量导入导出，便于与其他文献管理工具或爬虫系统对接。
- **全文检索接口** 提供标题、描述与标签组合的轻量级全文搜索，支持模糊匹配与布尔组合。
- **访问统计埋点** 可选集成简单的访问计数与来源分析，便于资源热度评估。
- **用户自定义视图** 允许按标签、日期或状态筛选生成自定义列表页，并支持保存为个人视图配置。

## 应用场景

- **技术文档维护者** 在维护大型软件文档时，常需要引用外部标准、RFC 文档或第三方库主页。NexusIndex 可集中管理这些引用，并自动检测失效链接，避免文档中出现死链。
- **社区内容运营** 社区周报或资源月刊编辑可利用 NexusIndex 建立投稿链接暂存池，在审核后统一生成公开列表页，减少手工整理与排版成本。
- **个人知识库扩展** 研究员或工程师可将日常浏览发现的优质技术博文、视频教程与开源仓库统一收录，并通过标签进行主题归类，便于后续检索与回顾。
- **合规审计追踪** 法务或合规团队可对外部引用资源进行定期复核，通过状态监控与备注字段记录合规结论，确保对外发布内容符合监管要求。

## 快速开始

以下步骤适用于 Linux 与 macOS 环境，Windows 用户建议使用 WSL 或 Git Bash。

```bash
# 克隆项目仓库
git clone https://github.com/nexusindex/nexusindex.git
cd nexusindex

# 安装依赖（使用 pip 与虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 初始化本地条目库并启动开发服务器
nexusindex init --path ./entries
nexusindex serve --port 8080
```

执行完成后，打开浏览器访问 <code>http://localhost:8080</code> 即可查看默认导航页面。

## 安装要求

| 依赖 | 必需版本 | 说明 |
|---|---|---|
| Python | >= 3.9 | 核心运行时，用于 CLI 工具与构建脚本 |
| pip | >= 21.0 | 包管理器，用于安装项目依赖 |
| Git | >= 2.25 | 版本控制，用于克隆仓库与拉取更新 |
| Markdown | >= 3.3 | 用于解析条目描述字段中的轻量标记 |
| PyYAML | >= 6.0 | 用于支持 YAML 格式的元数据头信息 |
| requests | >= 2.28 | 用于链接可用性检测的 HTTP 客户端 |
| beautifulsoup4 | >= 4.11 | 用于静态页面生成时的 HTML 结构处理 |
| watchdog | >= 2.1 | 开发模式下用于文件变更自动重载 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户指南 | /docs/user-guide/ | 如何添加、编辑、删除资源条目；如何创建标签与视图 |
| 运维手册 | /docs/operations/ | 如何部署生产环境；如何配置反向代理与 SSL 证书 |
| 开发者文档 | /docs/developer/ | 如何扩展自定义检测器；如何修改静态主题模板 |
| API 参考 | /docs/api/ | 导入导出接口定义；插件钩子说明 |
| 变更日志 | /docs/changelog/ | 每个版本的新增功能、修复与破坏性变更 |
| 设计说明 | /docs/design/ | 索引数据结构设计、缓存策略与性能考量 |

## 资源列表

### 直播与视频内容相关

<code>https://guochanwanghongzhibozhuzaixian.org.cn</code>

<code>https://guochanwanghongshipinzhibo.org.cn</code>

<code>https://wanghongzhibomianfeiguankan.org.cn</code>

<code>https://meinvzhibozaixiankan.org.cn</code>

<code>https://guochanwanghongfulishipin.org.cn</code>

<code>https://rihanzhibofulishipin.org.cn</code>

<code>https://rewuzhibowanghongzhibo.org.cn</code>

## 项目结构

```
nexusindex/
├── cli/                          # 命令行入口模块
│   ├── parser.py                 # 参数解析与子命令路由
│   └── commands/                 # 各子命令实现 (init, serve, build, check)
├── core/                         # 核心数据模型与索引引擎
│   ├── models/                   # 资源条目、标签、视图的数据类定义
│   ├── storage/                  # 文件系统读写抽象层，支持 JSON/YAML
│   └── indexer.py                # 索引构建与查询核心逻辑
├── monitor/                      # 链接状态监控子模块
│   ├── checker.py                # 异步 HTTP 状态检测器
│   ├── cache.py                  # 状态结果缓存，避免重复请求
│   └── notifier.py               # 状态变更时的通知接口（邮件/Webhook）
├── generator/                    # 静态站点生成器
│   ├── theme/                    # 默认主题模板（HTML + CSS + JS）
│   ├── page_builder.py           # 列表页、详情页、标签页的构建器
│   └── assets/                   # 静态资源（图片、字体、样式表）
├── plugins/                      # 插件扩展目录，支持自定义检测与过滤
│   ├── example_plugin.py         # 示例插件，展示钩子用法
│   └── validator/                # 针对特定网站类型的校验器
├── tests/                        # 单元测试与集成测试用例
│   ├── test_models.py
│   ├── test_checker.py
│   └── fixtures/                 # 测试用样例数据
├── docs/                         # 完整文档源码（Markdown + MkDocs）
├── entries/                      # 用户资源条目存储目录（默认为空，需初始化）
├── requirements.txt              # 生产依赖清单
├── setup.py                      # 包安装配置
└── README.md                     # 本文件
```

## 贡献指南

1. 在 GitHub 上 fork 本项目，并克隆至本地开发环境。创建新分支时请使用 `feature/` 或 `fix/` 前缀，并关联对应 issue 编号。
2. 编写新功能或修复缺陷后，确保所有现有单元测试通过，并为新增逻辑补充至少一个测试用例。测试文件位于 `tests/` 目录，执行 `pytest` 即可运行全部测试。
3. 若涉及主题模板或静态资源变更，请同时更新 `docs/design/` 中的相关说明文档，并执行 `nexusindex build` 验证生产输出。
4. 提交代码前请运行 `flake8` 与 `black` 进行风格检查，确保代码符合 PEP 8 规范。提交信息格式建议为 `<type>(<scope>): <subject>`，例如 `feat(monitor): add retry policy for 5xx errors`。
5. 发起 Pull Request 至 `main` 分支，并在描述中明确列出变更内容、影响范围以及测试结果摘要。维护者将在 3 个工作日内进行审核。

## 常见问题

**问：检测链接可用性时是否会频繁触发目标网站限流？**

答：NexusIndex 默认采用被动检测策略，仅在页面渲染或手动触发检查时发送请求，且全局并发数限制为 4，单次检查间隔为 500 毫秒。同时支持配置 `--respect-robots` 参数，强制读取目标网站的 `robots.txt` 并遵守 `Crawl-delay` 指令。用户亦可设置白名单或黑名单域名，以跳过特定站点的检测。

**问：如何迁移现有书签或收藏夹数据至 NexusIndex？**

答：项目内置了 `import` 子命令，当前支持 Netscape HTML 书签导出格式（常见于浏览器导出功能）以及通用 CSV 格式（列映射可配置）。执行 `nexusindex import --format netscape --file bookmarks.html` 即可自动解析并生成条目。若需自定义映射规则，可参考 `docs/user-guide/import.md` 中的配置示例。

**问：静态站点是否支持多语言界面？**

答：自版本 2.1.0 起，主题模板已集成 i18n 支持，默认提供中文与英文语言包。用户可通过配置 `config.yaml` 中的 `locale` 字段切换界面语言。条目描述内容本身仍以创建时写入的语言为准，但标签、导航文字与状态提示均会按选定语言渲染。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
