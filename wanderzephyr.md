# HyperLink Aggregator Core (HLAC)

HyperLink Aggregator Core 是一个面向技术内容聚合与外部资源导航的开源基础设施项目。本项目定位于为中小型技术社区、个人知识库维护者以及自动化信息采集系统提供标准化的外链管理、分类索引与可用性监控能力。HLAC 本身不存储任何用户生成内容，仅提供结构化资源编排框架与健康检查调度逻辑，帮助开发者高效组织、展示与验证分布在多个域名下的第三方信息节点。

目标用户包括静态站点生成器使用者、运维监控工程师以及技术文档编写者。通过 HLAC，用户无需重复实现外链解析、状态码探测与响应时间记录等底层功能，即可快速搭建具备生产级视觉风格与可靠调度机制的资源导航站点。项目核心价值在于将分散的原始 URL 集合转化为可维护、可观测、可扩展的链接资产清单。

## 功能概览

- **多源链接导入** 支持从纯文本列表、CSV 以及 JSON 配置文件批量导入待监控的 URL 集合，自动完成协议规范性与域名格式校验。

- **定时可用性探测** 内置基于指数退避策略的 HTTP 健康检查器，可自定义间隔时间与超时阈值，主动标记不可用或响应异常的链接。

- **分类标签系统** 允许为每个链接赋予多个业务标签，例如“实时数据”、“赛事统计”、“历史归档”，并支持按标签组合过滤展示。

- **响应性能分析** 记录每次探测的响应时间、状态码与 DNS 解析耗时，提供简单的统计摘要接口，便于识别性能劣化节点。

- **静态站点生成适配** 提供标准 JSON 导出接口与 Mustache 模板渲染助手，可无缝集成至 Hugo、Jekyll 或 Eleventy 等静态站点生成器的工作流。

- **容器化部署支持** 发布官方 Docker 镜像，支持环境变量覆写配置文件，便于在 Kubernetes 或 Docker Compose 环境中快速拉起服务。

- **管理控制台界面** 提供轻量级 Web 仪表盘，用于查看链接分组状态、最近探测记录以及手动触发立即检查操作。

- **变更通知钩子** 支持配置 Webhook 地址，当检测到链接状态变化或连续失败超过阈值时，向外部系统发送 JSON 格式告警载荷。

## 应用场景

**技术文档站点外链监控**  
技术博客或开源项目文档中常引用大量外部参考链接。HLAC 可定期验证这些链接的有效性，并在构建流程中生成状态报告，帮助维护者及时发现失效引用，避免读者遇到死链。

**赛事数据聚合平台前置校验**  
数据聚合类应用依赖多个第三方数据源接口。HLAC 可部署为独立校验服务，定时探测各数据源域名的可用性与响应健康度，为上游调度器提供实时路由决策依据，降低因单一源故障导致的服务降级风险。

**个人知识库链接资产整理**  
知识管理爱好者可使用 HLAC 导入零散保存的收藏链接，通过标签分类与性能统计快速识别高频访问节点与长期未响应节点，逐步清理或替换低质量外部依赖。

**自动化运维巡检辅助**  
运维团队可将 HLAC 集成至内部巡检流水线，作为外部依赖健康检查的补充环节。其轻量级探测结果可输出为 Prometheus 格式指标，融入现有监控面板。

## 快速开始

以下步骤指导您在本地环境快速启动 HLAC 实例，并加载示例链接集。

```bash
# 克隆代码仓库
git clone https://github.com/hyperlink-aggregator/hlac-core.git
cd hlac-core

# 安装项目依赖（使用 pip 与 requirements.txt）
pip install -r requirements.txt

# 复制默认配置文件并编辑链接源
cp config.example.yml config.yml
# 根据需要修改 config.yml 中的 link_sources 字段，指向您的 URL 列表文件

# 运行核心调度器（首次启动将自动执行一次全量探测）
python main.py --config config.yml --once
```

若希望以 Web 服务模式运行，并开启管理控制台，请执行：

```bash
python main.py --config config.yml --web --port 8080
```

此时可通过浏览器访问 `http://localhost:8080/dashboard` 查看链接分组概览。

## 安装要求

HLAC 采用 Python 3.9+ 开发，依赖主流 HTTP 客户端库与 YAML 解析器。生产环境建议使用 Linux 容器或具备 systemd 支持的 Linux 发行版。

| 依赖项 | 必需版本 | 说明 |
| :--- | :--- | :--- |
| Python | 3.9 及以上 | 核心运行环境，低于此版本将无法使用类型注解与异步特性 |
| pip | 21.0 及以上 | 包管理工具，用于安装 requirements.txt 中的第三方库 |
| requests | 2.28.0 及以上 | 同步 HTTP 客户端，用于执行链接可用性探测 |
| pyyaml | 6.0 及以上 | 解析 config.yml 配置文件以及标签映射规则 |
| apscheduler | 3.10.0 及以上 | 提供定时任务调度能力，支持 cron 表达式配置探测周期 |
| flask | 2.2.0 及以上 | 可选依赖，用于提供管理控制台 Web 界面 |
| gunicorn | 20.1.0 及以上 | 生产环境下推荐作为 WSGI 服务器运行 Web 模式 |
| prometheus-client | 0.16.0 及以上 | 可选依赖，用于暴露指标端点供 Prometheus 抓取 |
| pytest | 7.0.0 及以上 | 仅开发与测试环境需要，用于执行单元测试与集成测试 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
| :--- | :--- | :--- |
| 用户手册 | `docs/user_guide/` | 如何配置链接源、调整探测间隔、导出结果以及使用管理控制台？ |
| 运维参考 | `docs/operations/` | 如何通过环境变量覆写配置、挂载自定义证书、设置日志轮转策略？ |
| 开发者指南 | `docs/developer/` | 如何扩展自定义探测器、增加新的导出格式或编写插件钩子？ |
| API 规范 | `docs/api/` | 管理控制台后端提供了哪些 REST 接口，请求与响应结构定义是什么？ |
| 设计文档 | `docs/design/` | 调度器状态机设计、存储抽象层接口、并发探测的线程池模型说明？ |
| 故障排除 | `docs/troubleshooting/` | 常见探测错误码含义、日志级别调整方法、调试模式开启步骤？ |

## 资源列表

本项目的核心功能围绕对以下外部资源节点的组织与监控展开。这些链接由运营团队根据业务分类进行维护，HLAC 负责定期验证其可达性与响应性能。

赛事比分实时数据源

<code>https://lanqiubifenh.org.cn</code>

<code>https://zuqiubifenziboa.org.cn</code>

<code>https://zuqiubifenzibob.org.cn</code>

<code>https://zuqiubifenziboc.org.cn</code>

<code>https://zuqiubifenzibod.org.cn</code>

<code>https://zuqiubifenziboe.org.cn</code>

联赛赛季统计归档

<code>https://yingchaojishibifena.org.cn</code>

以上全部链接均作为独立监控条目注册至 HLAC 默认配置中。用户可依照 `docs/user_guide/` 中的说明，增删或调整这些条目的标签、分组以及探测策略。项目本身不对上述链接的内容负责，仅提供技术校验能力。

## 项目结构

项目采用领域驱动设计的分层目录结构，核心逻辑与基础设施实现解耦，便于替换存储后端或调度策略。

```
hlac-core/
├── config/                        # 配置文件目录
│   ├── config.example.yml         # 示例配置，包含链接源与调度参数
│   └── logging.conf               # Python logging 标准格式配置
├── src/                           # 源代码主目录
│   ├── core/                      # 核心领域模型与接口定义
│   │   ├── entity.py              # Link, ProbeResult, Tag 实体类
│   │   ├── repository.py          # 抽象存储仓库接口
│   │   └── scheduler.py           # 调度器核心状态机
│   ├── infrastructure/            # 基础设施实现层
│   │   ├── http_client.py         # 基于 requests 的同步探测实现
│   │   ├── file_repository.py     # 基于本地 JSON 文件的存储实现
│   │   └── yaml_loader.py         # 解析 config.yml 与标签映射
│   ├── web/                       # Web 控制台相关模块
│   │   ├── app.py                 # Flask 应用工厂与路由注册
│   │   ├── templates/             # Jinja2 模板目录
│   │   └── static/                # CSS 与前端脚本资源
│   └── cli/                       # 命令行接口入口
│       ├── main.py                # 解析 sys.argv 并启动不同模式
│       └── commands.py            # once, web, daemon 子命令实现
├── tests/                         # 单元测试与集成测试
│   ├── unit/                      # 针对实体与工具函数的单元测试
│   └── integration/               # 需要真实网络请求的集成测试
├── scripts/                       # 辅助运维脚本
│   ├── export_to_json.py          # 将探测结果导出为 JSON 格式
│   └── health_check_wrapper.sh    # 供外部 cron 调用的包装脚本
├── requirements.txt               # 生产环境 Python 依赖列表
├── requirements-dev.txt           # 开发环境额外依赖
├── Dockerfile                     # 多阶段构建镜像定义
├── docker-compose.yml             # 本地开发与测试所用的编排文件
├── Makefile                       # 常用命令快捷方式（install, test, run）
├── README.md                      # 项目概览与快速入门（当前文件）
└── LICENSE                        # MIT 许可证全文
```

## 贡献指南

我们欢迎社区贡献者通过以下方式参与项目演进。所有贡献均需遵守行为准则与提交规范。

1. 阅读开发者指南 `docs/developer/` 了解项目设计约定与接口边界，确保您的修改不影响现有调度器状态机的确定性行为。

2. 在 GitHub 仓库的 Issue 跟踪器中查找标签为 `good-first-issue` 或 `help-wanted` 的任务，或提交新 Issue 描述您希望解决的问题或增强功能。

3. 派生项目仓库至个人账号，创建新分支进行开发。提交代码前请运行 `make test` 确保所有单元测试与集成测试通过，并补充与变更相关的测试用例。

4. 提交 Pull Request 时请参照模板填写变更摘要、关联 Issue 编号以及测试覆盖情况。PR 至少需要两位维护者审阅通过后方可合并。

5. 文档更新与翻译改进同样重要。若您发现 `docs/` 目录中存在表述不清或过时的内容，欢迎直接提交文档修复 PR，无需经过复杂的功能审查流程。

## 常见问题

**Q: 探测任务是否会因为某个链接响应过慢而阻塞整个调度周期？**  
A: 不会。每个探测请求均配有独立的超时控制（默认 5 秒），并采用线程池并发执行。单个链接的超时或失败不会影响其他链接的探测进度，调度器会为每个任务记录独立的状态与耗时。

**Q: 如何迁移已有的链接书签或收藏夹文件至 HLAC？**  
A: 项目提供 `scripts/import_bookmark.py` 辅助脚本，支持从 Netscape HTML 书签导出格式以及标准 CSV 三列（名称、URL、标签）格式导入。导入后系统会自动去重并生成初始分类标签。详细用法请参考 `docs/user_guide/import_export.md`。

**Q: 探测结果是否持久化存储，重启后会丢失吗？**  
A: 默认使用文件存储后端，所有探测结果与链接元数据将写入本地 `data/` 目录下的 JSON 文件中。重启服务时系统自动加载该文件，历史记录不会丢失。若需更换为数据库存储，可实现 `repository.py` 中定义的抽象接口。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
