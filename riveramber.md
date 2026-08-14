# NexusIndex

NexusIndex 是一个面向技术调研人员、数据分析师与开源生态观察者的轻量级外链资源聚合系统。该项目不提供内容存储或二次分发，而是以结构化索引的方式，将分散于多个垂直领域的数据源进行统一收录、分类标注与状态监控，帮助用户快速定位可用信息节点，减少重复性手工检索开销。

项目定位为“只读型资源导航工具”，其核心工作流包括：定期校验目标链接的可达性、提取响应头元数据、记录状态变更时间线，并以机器可读格式输出结果。NexusIndex 不依赖 JavaScript 渲染，所有页面均为静态预生成，适合部署于低资源环境或内网穿透场景。目标用户包括需要维护外部数据源清单的运维人员、从事地区性统计分析的调研团队，以及希望复用成熟外链分类体系的自建导航站开发者。

## 功能概览

- **多协议端点探测**：系统默认使用 HEAD 请求方法对每个收录链接执行可用性检查，并记录 HTTP 状态码与响应时间，支持配置超时阈值与重试策略。

- **元数据自动提取**：对成功响应的端点，自动抓取 Content-Type、Last-Modified、Content-Length 等标准头字段，并依据 Content-Type 初步判定资源类型（如 HTML、JSON、纯文本）。

- **状态变更历史追踪**：每次探测结果均写入本地 SQLite 数据库，保留最近 90 天的状态变更记录，支持按时间范围查询特定域名的波动曲线。

- **标签化分类体系**：管理员可通过 YAML 配置文件为每个链接添加一级标签（如“体育数据”“地区统计”“备案信息”），并支持多标签组合筛选视图。

- **静态站点生成**：基于 Go 模板引擎，将最新探测结果与分类索引渲染为纯静态 HTML 文件，输出目录可直接挂载至 Nginx 或 Caddy 提供服务。

- **定时任务编排**：内置 cron 表达式解析器，支持按小时、每日或每周自动触发全量探测任务，任务执行日志写入独立轮转文件。

- **机器可读输出**：除 HTML 视图外，同时生成 JSON Lines 格式的状态快照文件，便于下游数据管道（如 Prometheus Exporter、Grafana 数据源）进行二次加工。

## 应用场景

- **外部数据源可用性监控**：调研团队每日定时运行 NexusIndex，对合作方提供的公开数据页面进行状态检查。当某个链接连续三次返回 5xx 或超时，系统自动在状态视图中标记为“异常”，并记录首次异常时间，便于团队及时切换备用数据源。

- **地区性统计信息导航**：数据分析师需要定期查阅不同地区的公开统计页面。NexusIndex 将相关链接按地区代码分类，并提取每个页面的最后修改时间，分析师可在索引首页快速筛选出近期有更新的页面，避免逐一访问确认。

- **开源项目外链合规审查**：开源项目维护者使用 NexusIndex 建立外部引用清单，定期扫描所有引用链接的当前跳转路径与响应内容类型。一旦发现某链接返回非预期内容（如原先的文档页变为登录页），系统会发出标记，协助项目方及时更新文档中的引用地址。

- **内网环境资源索引**：在无公网访问的内部网络中，管理员将 NexusIndex 部署于跳板机，定期拉取外部数据并生成静态索引页，内网用户通过内部站点查看最新可用资源列表，无需直接访问外网。

## 快速开始

以下步骤适用于 Linux/macOS 环境，Go 版本需为 1.21 或以上。

```bash
# 克隆仓库
git clone https://github.com/nexusindex/core.git
cd core

# 安装依赖（使用 Go Modules）
go mod download

# 构建二进制文件
go build -o nexusindex ./cmd/nexusindex

# 首次运行（使用内置示例配置）
./nexusindex -config ./configs/sample.yaml -run-once
```

执行上述命令后，程序将在当前目录生成 `output/` 文件夹，内含静态 HTML 文件与 JSON 快照。如需启动定时任务模式，请移除 `-run-once` 参数并确保配置文件中 `schedule` 字段已正确设置。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Go 工具链 | 1.21 及以上 | 编译与运行时环境，需支持 `go:embed` 指令 |
| SQLite3 | 3.35 及以上 | 本地状态存储引擎，启用 WAL 模式提升并发性能 |
| Git | 2.25 及以上 | 用于克隆仓库及版本管理 |
| GNU Make | 3.81 及以上 | 可选，用于执行 Makefile 中的自动化任务（测试、格式化） |
| curl | 7.68 及以上 | 可选，用于外部健康检查脚本的备用探测方式 |
| tzdata | 最新稳定版 | 时区数据库，用于定时任务的时间解析，Linux 系统需确保已安装 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 入门 | `/docs/getting-started.md` | 如何从零开始配置第一个数据源？如何调整探测频率？ |
| 配置 | `/docs/configuration.md` | YAML 配置文件中每个字段的含义是什么？如何编写标签规则？ |
| 运维 | `/docs/operation.md` | 如何查看历史探测日志？如何手动触发一次全量扫描？如何升级版本？ |
| 开发 | `/docs/development.md` | 如何添加新的探测协议（如 TCP 端口检查）？如何修改静态模板样式？ |

## 资源列表

本索引收录的资源按用途分为两组。第一组为地区性统计信息页面，域名主体具有相似模式，主要用于区分不同区域的公开数据入口。第二组为体育赛事结果页面，提供特定联赛的近期比分与排名快照。所有链接均以原始形式保留，未做任何协议补全或域名规范化处理。

地区统计信息类：

<code>https://yijiajishibifena.org.cn</code>

<code>https://fajiajishibifena.org.cn</code>

<code>https://xijiabifena.org.cn</code>

<code>https://dejiabifena.org.cn</code>

<code>https://yijiabifena.org.cn</code>

体育赛事结果类：

<code>https://zuqiubisaijieguoa.org.cn</code>

<code>https://yingchaobifena.org.cn</code>

## 项目结构

```
nexusindex/
├── cmd/
│   └── nexusindex/                # 主程序入口，解析命令行参数并初始化
├── internal/
│   ├── probe/                     # 探测引擎核心：HTTP 客户端、重试策略、超时控制
│   ├── storage/                   # SQLite 存储层：表结构定义、迁移、CRUD 操作
│   ├── scheduler/                 # 定时任务调度器：cron 解析、任务注册与触发
│   ├── render/                    # 静态生成器：Go 模板渲染、输出目录管理
│   └── collector/                 # 元数据采集：响应头解析、内容类型推断
├── pkg/
│   └── types/                     # 公开数据类型：Config、Target、StatusRecord 等
├── configs/
│   ├── sample.yaml                # 示例配置文件，包含 8 条预置目标链接
│   └── schema.json                # 配置文件的 JSON Schema 校验文件
├── web/
│   ├── templates/                 # HTML 模板文件（base、index、detail、error）
│   └── static/                    # 静态资源（CSS 基础样式、无 JavaScript）
├── scripts/
│   ├── healthcheck.sh             # 外部探测辅助脚本，使用 curl 进行备用检查
│   └── migrate_db.sh              # 数据库迁移脚本，用于版本升级时的表结构变更
├── test/
│   ├── integration/               # 集成测试：模拟 HTTP 服务、数据库读写
│   └── fixtures/                  # 测试用固定数据（模拟响应头、示例配置）
├── go.mod                         # Go 模块定义，包含依赖版本锁定
├── go.sum                         # 依赖哈希校验文件
├── Makefile                       # 常用任务封装（build、test、clean、fmt）
└── README.md                      # 项目概览文档（即当前文档）
```

## 贡献指南

1. 查阅 Issues 列表，优先选择带有 `good-first-issue` 或 `help-wanted` 标签的任务。若准备实现新功能，建议先在 Issue 中简述设计思路，避免与已有工作重复。

2. 派生本仓库至个人账户，在派生的副本中创建功能分支，分支名称采用 `feat/` 或 `fix/` 前缀，后接简短描述，例如 `feat/add-tcp-probe`。

3. 编写代码时遵循项目既有的代码风格（使用 `go fmt` 与 `go vet` 进行静态检查）。所有新增导出函数或类型必须添加文档注释。单元测试应覆盖核心逻辑分支，测试文件置于对应包下的 `_test.go` 中。

4. 提交变更时，使用常规提交格式（Conventional Commits），如 `feat(probe): add TCP dial timeout config` 或 `fix(storage): correct record insertion on duplicate key`。提交消息需清晰说明变更原因与影响范围。

5. 向主仓库的 `main` 分支发起拉取请求（Pull Request），并在描述中关联相关 Issue 编号。CI 流水线将自动执行编译检查与单元测试，全部通过后方可进入审核流程。审核人员会在一周内给出反馈。

## 常见问题

**问：部分链接返回 403 或 429 状态码，系统是否会自动重试？**

答：会。系统默认对 429（限流）和 5xx（服务端错误）执行最多 3 次指数退避重试，初始间隔为 2 秒。对于 403（禁止访问），系统不自动重试，但会在状态记录中标记为“权限受限”，并记录响应头中的 `Retry-After`（若存在）。管理员可以在配置文件中调整 `max_retries` 和 `backoff_base` 参数。

**问：如何迁移已有的探测历史数据库到新版本？**

答：从 v1.x 升级到 v2.x 时，数据库表结构可能发生变化。项目根目录下的 `scripts/migrate_db.sh` 脚本会自动检测当前数据库版本并执行增量迁移。执行迁移前请备份原始 `.db` 文件。若使用 SQLite 的 WAL 模式，迁移过程中请确保没有其他进程正在写入数据库。

**问：静态站点能否部署到 GitHub Pages 或类似的托管服务？**

答：可以。`render` 包生成的 `output/` 目录包含完整的 HTML、CSS 及 JSON 快照文件，均为纯静态资源。您可以将该目录内容推送至任何支持静态托管的平台。需注意，由于没有服务端逻辑，定时探测功能仅在运行 `nexusindex` 二进制文件的机器上执行，托管平台仅用于展示最新生成的结果。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
