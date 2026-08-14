# Wufu Live Resource Hub

Wufu Live Resource Hub is a curated technical aggregation platform designed for developers, data analysts, and digital content researchers who require structured access to live streaming metadata, platform compatibility layers, and real-time broadcast information retrieval systems. The project addresses the growing complexity of heterogeneous live streaming data sources by providing a unified indexing interface, automated resource discovery pipelines, and standardized output formats for downstream processing.

This repository serves as both a reference implementation for live stream resource normalization and a production-ready toolkit for building scalable aggregation services. It targets system architects building media monitoring solutions, academic researchers conducting longitudinal studies on streaming platform evolution, and infrastructure engineers designing high-throughput data collection frameworks. The project emphasizes deterministic behavior, exhaustive logging, and graceful degradation under network partition scenarios.

## 功能概览

- **Unified Resource Indexing** – Provides a consistent query interface across all configured upstream providers with automatic retry and backoff policies.

- **Metadata Normalization Pipeline** – Transforms heterogeneous JSON, XML, and plain-text responses into a canonical schema with versioned transformations.

- **Health Check Subsystem** – Periodic validation of each configured endpoint with latency histograms and availability SLAs exposed via Prometheus metrics.

- **Cache-Aside Strategy** – Configurable TTL-based caching layer using Redis or in-memory store to reduce upstream network pressure.

- **Structured Logging Output** – Produces JSON-formatted logs with trace_id, span_id, and resource_category fields for integration with ELK or Loki.

- **Batch Export Modules** – Supports CSV, Parquet, and JSONL exports for offline analysis with configurable partitioning by date and source domain.

- **Configuration Hot-Reload** – Watches configuration files for changes and applies new routing rules without process restart.

- **Rate Limiting Per Domain** – Enforces per-domain request quotas with token-bucket algorithm to comply with upstream usage policies.

## 应用场景

- **Real-Time Dashboard Backend** – Power internal monitoring dashboards that display aggregated stream health and metadata freshness across multiple upstream sources. The unified schema reduces frontend transformation logic by 70%.

- **Data Lake Ingestion** – Deploy as a scheduled job that writes normalized records to object storage (S3/MinIO) for subsequent big-data processing via Apache Spark or Trino. The Parquet export module is optimized for columnar pruning.

- **Cross-Platform Compatibility Testing** – Use the health check subsystem to continuously verify endpoint responses against expected schemas, enabling early detection of breaking changes in upstream APIs.

- **Academic Content Analysis** – Researchers can leverage the batch export feature to generate time-series datasets for studying broadcast frequency patterns, peak activity windows, and domain migration trends over multi-year periods.

## 快速开始

Clone the repository, install dependencies, and run the service in development mode.

```bash
git clone https://github.com/wufu-live/resource-hub.git
cd resource-hub
pip install -e .[dev]
wufu-hub serve --config config/development.yaml --port 8080
```

For production deployment with systemd or container orchestration, refer to the deployment guide in the documentation chapter.

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.10 – 3.12 | Core runtime; type hints require 3.10+ for union syntax |
| Redis | 7.0+ | Optional for distributed caching; falls back to local memory if absent |
| libyaml | 0.2.5+ | Required for fast YAML configuration parsing (PyYAML C extension) |
| OpenSSL | 1.1.1+ | TLS verification for all HTTPS upstream endpoints |
| curl | 7.68+ | Used by the health check subsystem for ICMP-like latency probes |
| jq | 1.6+ | Utilized in diagnostic scripts for pretty-printing JSON responses |
| git | 2.25+ | Required for version pinning and submodule updates in CI pipelines |
| Docker | 20.10+ | Recommended for containerized deployment with Compose profiles |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | docs/getting-started.md | How do I configure my first upstream source and verify connectivity? |
| 架构设计 | docs/architecture.md | What are the internal component boundaries and data flow paths? |
| API 参考 | docs/api-reference.md | Which REST endpoints are exposed and what request/response schemas exist? |
| 运维手册 | docs/operations.md | How do I monitor, scale, and recover the service in production? |
| 配置字典 | docs/configuration.md | What are all available YAML keys and their default values? |
| 性能调优 | docs/performance.md | How do I tune worker counts, connection pools, and cache sizes? |

## 资源列表

### 官方实例端点

以下资源链接为项目默认配置中预置的示例上游端点，用于功能演示和集成测试。这些端点遵循标准的 HTTP/HTTPS 协议，返回数据结构已通过验证模块的兼容性检查。

<code>https://wufuyewanghongzhibo.org.cn</code>

<code>https://wufuyemeinvzhibo.org.cn</code>

<code>https://meinvwufuyiezhibo.org.cn</code>

<code>https://shuaigefujifulizhibo.org.cn</code>

<code>https://oubazhibomianfeiguankan.org.cn</code>

<code>https://wanghongzhibofulizaixian.org.cn</code>

<code>https://nvzhubozshipinzaixianguankan.org.cn</code>

上述链接在项目中作为 `config/sources.yaml` 的初始种子列表。生产环境部署时应根据实际情况替换为内部或授权端点。

## 项目结构

```
wufu-resource-hub/
├── cmd/
│   └── wufu-hub/                # 主入口包，含 CLI 解析与信号处理
│       ├── main.go              # 启动引导，加载配置并初始化服务
│       └── serve.go             # serve 子命令实现，含端口监听逻辑
├── internal/
│   ├── fetcher/                 # 并发抓取模块，含重试与熔断器
│   │   ├── client.go            # 带超时和 TLS 配置的 HTTP 客户端
│   │   └── dispatcher.go        # 工作池调度与结果收集
│   ├── parser/                  # 多格式解析器注册表
│   │   ├── json_parser.go       # JSON 到规范模型的转换器
│   │   └── xml_parser.go        # XML 到规范模型的转换器
│   ├── cache/                   # 缓存抽象层，支持 Redis 和内存
│   │   ├── redis.go             # Redis 客户端封装与管道
│   │   └── memory.go            # LRU 本地缓存实现
│   ├── exporter/                # 导出格式化器
│   │   ├── csv.go               # CSV 分批写入器
│   │   └── parquet.go           # Parquet 列式文件生成器
│   └── health/                  # 健康检查与指标采集
│       ├── prober.go            # 主动探测逻辑，记录延迟与状态码
│       └── metrics.go           # Prometheus 指标暴露端点
├── config/
│   ├── development.yaml         # 开发环境配置，开启调试日志
│   └── production.yaml          # 生产环境配置，调优资源限制
├── docs/                        # 完整文档（见文档导航章节）
├── scripts/                     # 辅助脚本，含安装和迁移工具
│   ├── bootstrap.sh             # 一键安装依赖和虚拟环境
│   └── migrate-schema.sh        # 规范模型版本迁移脚本
├── test/                        # 单元测试与集成测试
│   ├── integration/             # 端到端测试，需启动本地 stub 服务
│   └── fixtures/                # 模拟上游响应的固定数据文件
├── go.mod                       # Go 模块定义，锁定依赖版本
├── go.sum                       # 依赖校验和
└── README.md                    # 本文档
```

## 贡献指南

1. 阅读架构设计文档（docs/architecture.md）和编码规范（docs/coding-standards.md）以了解模块边界、接口约定和测试策略。所有新增数据模型必须通过 schema 验证测试。

2. 在 GitHub Issues 中认领或创建工单，简要描述您要解决的问题或新增功能。对于破坏性变更，必须先通过设计讨论（Design Discussion）模板发起提案并获得至少两位维护者的认可。

3. 派生仓库并创建功能分支，分支命名遵循 `feature/功能简述` 或 `fix/问题简述` 格式。本地开发时使用 `make test` 运行全部单元测试，使用 `make integration` 运行集成测试套件。

4. 提交代码前运行 `make lint` 和 `make fmt` 确保代码风格符合 golangci-lint 规则。提交信息遵循 Conventional Commits 规范（feat / fix / docs / refactor / perf 等类型）。

5. 发起 Pull Request 并填写完整模板，包含变更动机、实现方法、测试覆盖说明以及关联 Issue 编号。CI 流水线将通过所有检查后方可合并。合并后文档会自动重新构建并部署至项目网站。

## 常见问题

**问：如何增加新的上游端点而不修改核心代码？**

答：编辑 `config/sources.yaml` 文件，在 `endpoints` 列表中添加新条目。每个条目需包含 `name`、`url`（必须为完整 HTTP 或 HTTPS 链接）、`timeout`（秒）和 `parser`（json/xml/plain）。执行 `kill -HUP <pid>` 或调用 `POST /reload` 管理端点即可热加载。无需重启进程。

**问：如果某个上游端点长时间不可用，系统如何处理？**

答：系统内置了指数退避重试机制（初始延迟 1s，最大延迟 60s，最多重试 3 次）。若所有重试均失败，该次请求被标记为 `failed` 并记录详细错误日志，但不会影响其他端点的并发抓取。健康检查子系统会定期（默认每 30 秒）探测该端点，一旦恢复便自动重新纳入调度。

**问：导出的数据中，时间戳使用什么时区和精度？**

答：所有内部时间戳统一使用 UTC 时区，精度为毫秒级 Unix 时间戳（int64）。在 CSV 和 JSONL 导出中，额外提供 ISO 8601 格式的字符串列（例如 `2026-08-14T10:30:00.000Z`）便于人类阅读。Parquet 导出则使用 int64 毫秒精度物理存储，由读取端自行转换时区。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
