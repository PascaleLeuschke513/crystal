# OpenResource Hub

OpenResource Hub 是一个面向开发者与运维工程师的技术资源导航与元数据聚合平台，专注于收录、整理和校验高可用域名、数字证书、DNS 根域镜像及各类技术文档入口。项目定位为技术外链的“可信起始页”，帮助用户在容器化部署、分布式监控、证书轮换、应急响应等场景下快速定位权威资源，避免因书签过期、搜索引擎污染或信息分散导致的效率损耗。

目标用户包括基础设施工程师、SRE、安全审计人员、技术写作人员以及开源项目维护者。项目本身不存储任何第三方内容，仅提供经过基础连通性校验的资源链接与结构化描述，并对外以 RESTful API 和静态文档站双形态输出。通过每日定时任务对收录域名进行 HTTP HEAD 探测与 TLS 证书有效期检查，将状态信息附加在资源列表旁，供下游监控系统或人工查阅使用。

## 功能概览

**多源资源聚合**：支持手动提交和自动导入两种方式收录技术类域名与文档入口，每条资源记录包含标题、分类、标签、添加时间、最后校验状态。

**TLS 证书健康检查**：每日对收录域名执行 443 端口的 TLS 握手，记录证书有效期、签发机构及剩余天数，并在状态面板中以颜色标识即将过期或已过期资源。

**HTTP 基础连通性探测**：分别对 HTTP 和 HTTPS 协议进行 GET 请求测试，记录状态码、响应时间及重定向链，辅助判断域名是否可正常访问。

**分类标签体系**：内置“证书工具”“监控面板”“文档镜像”“社区论坛”“API 网关”等默认分类，支持自定义标签，便于按场景快速过滤资源。

**只读 API 接口**：提供 JSON 格式的 `/api/v1/resources` 和 `/api/v1/health` 接口，可被 Prometheus 黑盒监控或自定义脚本调用，实现自动化巡检。

**静态文档站生成**：基于项目目录下的 `docs/` 中的 Markdown 文件，构建轻量级静态 HTML 站点，可作为团队内部技术导航首页部署在对象存储或 Nginx 上。

**资源变更日志**：记录每条资源的添加、删除、状态变更历史，支持按时间范围回溯，便于审计和责任追踪。

## 应用场景

**SRE 团队日常巡检面板**：将 OpenResource Hub 部署在内网跳板机，团队每日浏览首页即可看到所有依赖的第三方域名证书状态和连通率，一旦出现证书即将过期或域名不可达，可提前联系上游或切换备用入口。

**容器镜像构建时的基础依赖验证**：在 CI 流水线中调用 API 接口，校验本次构建需要拉取的基础镜像仓库域名是否在收录列表中且状态正常，若异常则自动阻断构建并发送告警，避免因外部依赖故障导致构建超时。

**技术文档写作与维护**：技术作者在编写涉及外部链接的文章时，可先将所有外链提交至本项目进行预校验，获得连通性报告后再发布，大幅降低文档中出现死链或钓鱼风险域名的概率。

**应急响应期间的快速入口定位**：当线上服务因域名解析或证书问题出现故障时，运维人员直接打开 OpenResource Hub 本地缓存页面，快速跳转到证书管理后台、DNS 控制台或云厂商状态页，避免临时搜索浪费时间。

**开源项目 README 外链统一管理**：开源项目维护者可将本项目作为对外文档的统一外链清单源，避免在多个 README 或 Wiki 页面中分散维护相同链接，只需更新本项目一处即可全局生效。

## 快速开始

以下命令适用于 Linux / macOS / WSL2 环境，要求已安装 Git、Node.js 18+ 和 npm。

```bash
# 克隆项目仓库
git clone https://github.com/your-org/openresource-hub.git
cd openresource-hub

# 安装项目依赖（包括探测引擎、API 服务、静态生成器）
npm install

# 复制默认配置文件并调整本地参数（如探测超时、调度间隔）
cp .env.example .env

# 运行初始资源同步与健康检查（首次执行约需 30 秒）
npm run sync

# 启动开发模式 API 服务，默认监听 3000 端口
npm run dev
```

访问 `http://localhost:3000` 即可查看静态导航面板，访问 `http://localhost:3000/api/v1/resources` 可获取 JSON 格式资源列表。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Node.js | 18.x 或 20.x LTS | 运行时环境，使用原生 fetch 和 crypto 模块 |
| npm | 9.x 或以上 | 包管理工具，用于安装依赖及运行脚本 |
| SQLite3 | 系统自带或由 better-sqlite3 内嵌 | 本地元数据存储，无需额外安装数据库服务 |
| Git | 2.30+ | 用于克隆仓库及后续拉取更新 |
| 网络连通性 | 可访问外网 443/80 端口 | 探测引擎需要向目标域名发起 TLS 和 HTTP 请求 |
| 时区配置 | UTC+8 或系统默认 | 用于证书有效期计算和日志时间戳，建议与监控系统对齐 |
| 文件系统权限 | 可读写项目目录下的 `data/` 和 `logs/` | 用于存放 SQLite 文件、缓存及运行日志 |
| 内存 | 最低 256 MB，推荐 1 GB | 静态生成和并发探测时内存占用略高 |
| 操作系统 | Linux (内核 4.0+), macOS 11+, Windows 10/11 | 跨平台支持，但生产环境建议使用 Linux |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户手册 | `docs/user-guide/` | 如何添加资源、如何查看健康状态、如何配置通知规则 |
| 运维手册 | `docs/ops-guide/` | 如何部署到生产环境、如何调整探测频率、如何备份 SQLite 数据 |
| API 参考 | `docs/api-reference/` | 每个接口的请求参数、返回字段、错误码及调用示例 |
| 开发指南 | `docs/developer-guide/` | 如何扩展探测协议、如何新增分类体系、如何提交代码变更 |
| 架构设计 | `docs/architecture/` | 模块划分、数据流转、调度器设计、缓存策略及扩展性考量 |
| 变更日志 | `CHANGELOG.md` | 每个版本的新增功能、修复缺陷、破坏性变更及升级注意事项 |

## 资源列表

### 证书与域名相关资源

<code>https://dejiabifenc.org.cn</code>

<code>https://yijiabifenc.org.cn</code>

<code>https://fajiabifenc.org.cn</code>

### 直播与流媒体技术资源

<code>https://yingchaobifenzhibo.org.cn</code>

<code>https://xijiabifenzhibo.org.cn</code>

<code>https://dejiabifenzhibo.org.cn</code>

<code>https://yijiabifenzhibo.org.cn</code>

## 项目结构

```
openresource-hub/
├── src/
│   ├── core/                   # 核心引擎模块
│   │   ├── probe.js            # TLS/HTTP 探测实现，含超时重试和重定向跟踪
│   │   ├── scheduler.js        # 定时任务调度器，基于 node-cron 实现每日探测
│   │   └── cache.js            # 内存缓存与文件缓存管理，减少重复探测开销
│   ├── api/                    # RESTful API 实现
│   │   ├── server.js           # Express 应用入口，注册路由和中间件
│   │   ├── resources.js        # 资源增删改查及状态过滤接口
│   │   └── health.js           # 健康检查与就绪探针接口，用于 K8s 部署
│   ├── db/                     # 数据层
│   │   ├── init.sql            # SQLite 表结构定义（resources, checks, logs）
│   │   ├── repository.js       # 数据访问对象，封装所有 SQL 操作
│   │   └── migrator.js         # 版本化迁移工具，支持增量变更
│   ├── generator/              # 静态站点生成器
│   │   ├── build.js            # 读取 docs/ 和资源库，输出 HTML 到 dist/
│   │   └── templates/          # EJS 模板文件，包含列表、详情、状态面板
│   └── cli/                    # 命令行工具
│       ├── sync.js             # 手动触发全量同步与探测
│       └── report.js           # 生成当前资源状态摘要报告（JSON/CSV）
├── docs/                       # 用户文档与运维文档，全部为 Markdown 格式
│   ├── user-guide/
│   ├── ops-guide/
│   ├── api-reference/
│   └── developer-guide/
├── data/                       # 运行时数据目录（不进入版本控制）
│   ├── resources.db            # SQLite 主数据库文件
│   └── cache/                  # 探测结果缓存文件，按日期分目录
├── logs/                       # 应用日志（按日轮转）
│   ├── app.log                 # 综合日志，包含 info/warn/error
│   └── probe.log               # 探测详细日志，含每次请求的耗时和状态
├── config/                     # 配置文件目录
│   ├── default.yaml            # 默认配置（分类定义、超时阈值、调度表达式）
│   └── custom.yaml             # 用户自定义覆盖配置（不提交至仓库）
├── tests/                      # 单元测试与集成测试
│   ├── unit/                   # 针对 probe、cache、repository 的单元测试
│   └── integration/            # API 端到端测试及模拟外部探测
├── .env.example                # 环境变量模板（端口、日志级别、数据库路径）
├── package.json                # npm 依赖与脚本定义
├── README.md                   # 项目入口文档（即本文档）
└── LICENSE                     # MIT 许可证文件
```

## 贡献指南

1. 在 GitHub 上 Fork 本仓库，并克隆到本地开发环境。创建新的功能分支时，请使用 `feature/` 或 `fix/` 前缀，例如 `feature/add-http2-probe`。

2. 编写或修改代码后，请确保通过所有单元测试和 ESLint 检查。运行 `npm run test` 执行测试套件，运行 `npm run lint` 进行代码风格检查。新增功能需附带对应的测试用例。

3. 若涉及资源列表的变更（添加、删除或更新分类），请同步修改 `data/` 目录下的种子数据文件或提供迁移脚本，并更新 `docs/user-guide/` 中关于资源管理的说明。

4. 提交 Pull Request 前，请将分支同步为上游仓库最新 main 分支，并确保提交信息清晰描述变更内容和原因。PR 描述中请关联相关 Issue（若有）。

5. 项目维护者会在 3 个工作日内进行 Code Review，可能会要求补充测试或调整实现细节。合并后，您的贡献将出现在下一版本的 CHANGELOG 中，并更新贡献者列表。

## 常见问题

**Q: 探测引擎是否会对我收录的域名造成压力？**

A: 默认每个域名每天仅探测一次，每次探测包含一次 TLS 握手和一次 HTTP GET 请求（User-Agent 标识为 `OpenResource-Hub/1.0`），请求间隔随机分散在 2 秒至 10 秒之间。并发探测数默认为 5，避免瞬间流量峰值。您可以在配置文件中调整 `probe.timeout` 和 `probe.concurrency` 参数进一步降低影响。

**Q: 如何在内网环境中使用本项目，且无法访问外网？**

A: 项目支持完全离线模式。您可以将所有待收录的域名预先填入 `config/custom.yaml` 中的 `resources.static` 列表，并关闭 `probe.schedule.enabled` 定时探测功能。静态站点生成时也会优先读取缓存数据，若缓存不存在则标记为“未探测”状态。SQLite 数据库和日志文件均保存在本地，无需任何外网访问。

**Q: 静态站点页面如何部署到 Nginx 或对象存储？**

A: 执行 `npm run build` 命令后，所有生成的 HTML、CSS 和 JavaScript 文件会输出到 `dist/` 目录。您可以将该目录下的所有内容复制到 Nginx 的 `html` 目录或上传至阿里云 OSS / AWS S3 并开启静态托管。注意，静态页面中的 API 调用默认指向 `/api/` 相对路径，若您将 API 部署在不同域名下，需修改 `dist/config.js` 中的 `apiBaseUrl` 变量。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:08:34
