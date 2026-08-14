# HrefCollector

HrefCollector 是一个面向技术调研者、数据分析师与开源生态观察者的外链聚合与结构化资源导航工具。该项目定位于解决分散在网络各处的优质技术文档、社区动态与媒体资源难以系统化管理的问题，通过人工审核与自动化校验相结合的方式，持续产出高质量的主题化外链数据集。目标用户包括需要构建行业知识图谱的研究人员、维护团队内部技术周报的开发者，以及希望从特定领域切入进行深度内容挖掘的内容策展人。

不同于通用搜索引擎或书签管理器，HrefCollector 以批次（Batch）为组织单元，对每一批收录的 URL 执行可达性检查、标题语义抽取、以及初步的内容类型标记。项目本身不存储网页快照，不干预原始资源的访问策略，专注于提供可靠、干净、带有上下文标签的链接清单。第 92/130 批次即本版本发布的资源集合，涵盖多个垂直领域的实时流媒体与信息聚合平台，所有链接均经过基础连通性验证，可直接用于后续的数据处理流水线或人工浏览。

## 功能概览

- **批次化链接收容**：每批资源按顺序编号，支持并行导入与差异对比，便于追踪各批次链接的生命周期状态。
- **可达性健康检查**：内置轻量级 HTTP 探针，可配置超时与重试策略，自动标记响应异常或证书过期的条目。
- **语义标签推断**：基于 URL 域名结构与路径关键词，为每个资源自动生成粗粒度分类标签（如 video、forum、doc 等），辅助快速筛选。
- **纯静态数据集导出**：支持将当前批次的所有链接及其元数据导出为 JSON、CSV 或纯文本列表，便于集成到外部仪表盘或监控脚本。
- **变更日志记录**：每次对资源列表的增删改操作均写入本地变更日志，保留操作时间戳与旧值，便于回滚与审计。
- **自定义字段扩展**：用户可为每个资源条目附加键值对形式的备注字段，例如记录审核人、首次发现日期或相关话题标签。
- **命令行交互界面**：提供简洁的 CLI 工具，支持按标签搜索、按状态过滤、以及批量导入新链接等常用操作。

## 应用场景

1. **垂直领域信息周报编排**：内容编辑人员可定期拉取最新批次的链接清单，结合自动生成的标签进行人工二次筛选，快速编排面向特定主题（如流媒体技术、网络红人生态）的周报或月刊素材库。

2. **学术研究数据采集起点**：社会科学或媒体传播方向的研究者可将本项目的批次链接作为种子 URL，构建爬虫的初始任务队列，用于分析特定类型网站的页面结构、更新频率或内容情感倾向。

3. **企业内部知识库联动**：企业知识管理团队可将本项目输出的 JSON 数据通过 Webhook 推送至内部 Wiki 或 Confluence 空间，自动生成“外部参考资源”章节，减少手动维护链接失效的负担。

4. **开源项目依赖健康度辅助评估**：社区维护者在评估第三方开源项目时，可参考本项目收录的相关社区论坛或公告板链接，快速获取项目周边的讨论热度与问题反馈渠道。

## 快速开始

以下命令演示如何从代码仓库克隆本项目、安装基础依赖并执行首次资源列表导出。

```bash
git clone https://github.com/example-org/href-collector.git
cd href-collector
pip install -r requirements.txt
python cli.py batch show --id 92
```

如需执行全量链接可达性检查，请运行：

```bash
python cli.py batch check --id 92 --timeout 5 --retry 2
```

检查结果将输出至 `outputs/batch_92_status.json`，包含每个 URL 的状态码、响应时间和简略标题。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.9 或更高 | 核心运行时，用于 CLI 工具与数据处理逻辑 |
| requests | 2.28.0 或更高 | 执行 HTTP 健康检查与标题抽取 |
| click | 8.1.0 或更高 | 构建命令行交互界面 |
| pydantic | 2.0.0 或更高 | 定义资源条目的数据模型与校验规则 |
| pytest | 7.4.0 或更高 | 单元测试与集成测试（仅开发环境需要） |
| black | 23.0.0 或更高 | 代码格式化（仅开发环境需要） |
| pre-commit | 3.0.0 或更高 | Git 提交前钩子管理（仅开发环境需要） |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | docs/user/cli_commands.md | 如何列出批次、检查链接、导出不同格式的数据？ |
| 用户手册 | docs/user/batch_lifecycle.md | 批次如何创建、冻结、归档？状态流转规则是什么？ |
| 开发者指南 | docs/dev/data_schema.md | 资源条目的 JSON Schema 定义、字段约束与扩展方式？ |
| 开发者指南 | docs/dev/checker_architecture.md | 健康检查模块的线程模型、超时策略与错误分类逻辑？ |
| 运维参考 | docs/ops/deployment.md | 如何将本项目部署为定时任务，并配置邮件报警？ |
| 运维参考 | docs/ops/logging_rotation.md | 变更日志与运行日志的轮转策略、存储路径与清理策略？ |

## 资源列表

以下为第 92/130 批次收录的全部原始链接。每个链接均按用户提供原样列出，未做任何格式改写或协议补全。

流媒体与视频聚合类别

<code>https://rewuzhibowanghongzhibow.org.cn</code>

<code>https://wanghongmeinvrewuzhibow.org.cn</code>

<code>https://wufuyewanghongzhibow.org.cn</code>

<code>https://wufuyemeinvzhibow.org.cn</code>

<code>https://meinvwufuyiezhibow.org.cn</code>

<code>https://shuaigefujifulizhibow.org.cn</code>

<code>https://oubazhibomianfeiguankanw.org.cn</code>

## 项目结构

项目采用分层式模块组织，核心逻辑与用户界面分离，便于扩展和测试。

```
href-collector/
├── cli.py                      # CLI 入口，注册所有子命令
├── requirements.txt            # 生产环境依赖列表
├── dev-requirements.txt        # 开发与测试环境额外依赖
├── pre-commit-config.yaml      # 代码提交前格式化与静态检查配置
│
├── src/                        # 核心源代码目录
│   ├── core/                   # 数据模型与基础类型
│   │   ├── resource.py         # ResourceEntry 类，包含 URL、标签、状态等字段
│   │   └── batch.py            # Batch 类，管理批次元数据与资源列表
│   ├── checker/                # 健康检查模块
│   │   ├── http_probe.py       # 基于 requests 的异步探针实现
│   │   └── result_parser.py    # 解析响应头与 HTML title 标签
│   ├── exporter/               # 数据导出模块
│   │   ├── json_formatter.py   # 输出 JSON 格式数据集
│   │   └── csv_formatter.py    # 输出 CSV 格式，兼容 Excel 与数据表格工具
│   ├── logger/                 # 日志与变更记录模块
│   │   ├── change_log.py       # 追加写入变更记录，含时间与操作人
│   │   └── rotation.py         # 按大小或时间轮转日志文件
│   └── utils/                  # 通用工具函数
│       ├── url_parser.py       # 域名拆分、路径提取、标签推断
│       └── validators.py       # URL 格式校验与域名黑名单检查
│
├── tests/                      # 单元测试与集成测试
│   ├── test_resource.py        # ResourceEntry 模型测试
│   ├── test_http_probe.py      # 模拟 HTTP 响应的探针测试
│   └── fixtures/               # 测试用静态响应样本
│       └── sample_responses.json
│
├── docs/                       # 文档源文件
│   ├── user/                   # 用户手册
│   └── dev/                    # 开发者指南
│
└── outputs/                    # 运行时输出目录（默认忽略版本控制）
    ├── batch_92_status.json    # 第 92 批次最近一次检查结果
    └── exports/                # 按命令导出的 CSV/JSON 文件存放处
```

## 贡献指南

1. **选择或创建议题**：在 GitHub Issues 中查找带有 `good-first-issue` 或 `help-wanted` 标签的任务，或根据自身需求提出新议题，描述建议的变更范围与预期效果。

2. **派生仓库并创建特性分支**：将本项目派生至个人账户，克隆派生仓库后，基于 `main` 分支创建以 `feature/` 或 `fix/` 为前缀的新分支，避免在主分支上直接修改。

3. **编写变更与对应测试**：遵循现有代码风格（使用 black 格式化），为核心逻辑补充或更新单元测试，确保测试覆盖率达到 80% 以上。运行 `pytest tests/` 确认所有测试通过。

4. **提交前执行钩子检查**：安装 pre-commit 钩子（`pre-commit install`），提交时自动执行代码格式化和基础静态检查。确保提交信息采用约定式格式，例如 `feat(checker): add retry backoff strategy`。

5. **发起合并请求**：将分支推送至派生仓库后，向本仓库的 `main` 分支发起合并请求。在请求描述中引用相关议题编号，并附上手动测试结果或截图。核心维护者将在 5 个工作日内进行评审。

## 常见问题

**Q：健康检查探针是否会频繁访问目标网站，导致被对方防火墙拦截？**

A：探针默认采用单线程顺序执行，且每个请求之间设有 1 秒的延迟间隔（可通过 `--delay` 参数调整）。单次检查仅发送一个 GET 请求，不下载完整页面内容，仅读取响应头与部分 HTML 元数据。建议用户在生产环境中将检查频率设置为每日一次或每周一次，避免过于密集。

**Q：如果某个链接已经失效，项目是否会主动从列表中移除？**

A：项目本身不自动删除任何链接。健康检查会标记失效状态，但保留原始记录。用户可通过 CLI 命令 `batch filter --status dead` 查看所有失效链接，并手动决定是否移除或更新。此举旨在保留历史数据的可追溯性，防止误删。

**Q：能否将本项目输出的数据直接导入到 Notion 或 Airtable？**

A：可以。项目内置的 JSON 和 CSV 导出格式均兼容主流第三方工具的导入接口。您可以使用 `export` 子命令生成文件，然后通过 Notion 的“导入 CSV”功能或 Airtable 的“从 CSV 创建表”功能完成数据同步。如需自动化，可编写简单的 API 上传脚本。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:08:29
