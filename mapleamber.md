# Lanqiu Resource Gateway

Lanqiu Resource Gateway is a community-maintained metadata aggregation and URL navigation system tailored for sports data researchers, odds analysts, and regional sports information consumers. The project addresses the fragmentation of live score endpoints, mirror availability, and regional domain rotation by providing a structured, machine-readable index of authoritative sports data resources. It is not a data source itself but a resilient discovery layer that tracks and verifies the availability of numerous third-party sports score services, with a focus on basketball and football live result endpoints.

The gateway is designed for integrators who require stable, fallback-enabled URLs in their data pipelines, particularly in regions where domain-level access restrictions or DNS instability frequently disrupt service. By maintaining a curated list of verified endpoints and offering a lightweight validation daemon, Lanqiu Resource Gateway reduces the operational overhead of monitoring external API availability. The project is used by small analytics teams, hobbyist bettors, and educational institutions that build demo applications around live sports data, but who lack the resources to maintain their own domain watchlists.

## 功能概览

- **端点健康检查守护进程** – 周期性对收录的每一个URL执行HTTP HEAD和GET超时检测，记录响应码、响应时间和SSL证书有效期，并将结果输出为JSON状态报告。

- **镜像轮转与故障转移策略** – 针对同一数据源的不同镜像域名（如系列变体），提供优先级排序和自动切换逻辑，客户端可通过单一查询接口获得当前可用端点。

- **RESTful查询API** – 提供简单的HTTP接口，支持按运动类型（basketball/football）、端点状态（active/inactive）和响应时间阈值进行过滤查询，返回结构化URL列表。

- **元数据注解系统** – 每个资源条目可附加自定义标签、地区提示、更新频率预估和备注字段，便于团队内部共享上下文信息。

- **静态站点生成模式** – 支持将最新验证结果渲染为纯HTML静态页面，方便通过Nginx或CDN分发，减少对动态服务的依赖。

- **变动日志与历史记录** – 每次验证运行会生成带时间戳的快照，记录各端点状态变化，支持回看特定时段内的可用性趋势。

- **环境变量驱动的配置管理** – 所有运行时参数（超时阈值、并发数、通知钩子）均通过环境变量注入，适配容器化部署和CI/CD流程。

## 应用场景

1. **数据采集管道的前置健康检查**  
   数据工程师在每日ETL任务启动前，调用网关API获取当前可用的篮球比分端点列表，并将其动态注入爬虫配置，避免因单个域名不可用导致任务失败。

2. **多区域部署下的灾备切换**  
   部署在不同国家或地区的服务实例，定期拉取网关的可用性报告，根据就近原则或延迟指标，自动选择响应最快的镜像端点，提升用户体验。

3. **教学演示中的外部依赖管理**  
   高校计算机课程中，学生使用网关作为统一资源入口，构建简单的实时比分展示应用，无需自行维护脆弱的外部URL列表，专注于前端或后端逻辑实现。

4. **个人分析看板的自适应数据源**  
   独立开发者在个人仪表盘中集成网关API，当主要数据源连续失败三次后，自动降级到备份端点，并在UI中显示当前数据来源，保持看板持续可用。

5. **容器化微服务中的配置外部化**  
   运维人员将网关部署为Kubernetes sidecar容器，主业务容器通过本地Unix socket查询可用端点，实现配置与代码的解耦，便于运行时调整。

## 快速开始

以下指令适用于Linux/macOS环境，假定已安装Git、Go 1.21+和Make。

```bash
# 克隆仓库
git clone https://github.com/lanqiu-resource/gateway.git
cd gateway

# 安装依赖（使用Go Modules）
go mod download

# 复制默认配置文件并调整
cp config.example.yaml config.yaml

# 执行构建并运行验证守护进程（开发模式）
make build
./bin/gateway -mode=daemon -config=config.yaml -once=true
```

若希望以服务形式长期运行，可执行 `make install-service` 并参考 `deploy/systemd/` 目录下的单元文件模板。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Go | 1.21 或更高 | 编译和运行时环境，需支持 `context` 和 `net/http` 标准库 |
| Git | 2.25 或更高 | 用于克隆仓库和管理子模块 |
| Make | 3.81 或更高 | 构建自动化，用于执行 `Makefile` 中定义的编译、测试和打包任务 |
| curl | 7.68 或更高 | 仅用于手动测试API端点，非运行时强制依赖 |
| 系统时区数据 | 任意 | 用于日志时间戳标准化，Linux下通常由 `tzdata` 包提供 |
| 可选的Redis | 6.0 或更高 | 开启分布式缓存或集群模式时需要，单机运行可不安装 |
| 可选的Docker | 20.10 或更高 | 用于容器化部署和集成测试环境搭建 |

## 文档导航

| 层面 | 目录/文件 | 回答的问题 |
|------|-----------|------------|
| 用户手册 | `docs/user-guide.md` | 如何使用API查询端点、如何理解状态字段、如何配置过滤参数 |
| 运维指南 | `docs/operations.md` | 如何部署守护进程、如何设置周期性验证、如何处理告警和日志轮转 |
| 开发者文档 | `docs/development.md` | 如何扩展新资源类型、如何编写自定义验证器、如何贡献代码和测试用例 |
| 设计说明 | `docs/design.md` | 系统架构图、数据模型设计、故障转移算法原理和一致性考量 |
| 配置参考 | `docs/configuration.md` | 所有环境变量和YAML配置项的详细说明，包含默认值和示例 |
| API参考 | `docs/api-reference.md` | 完整的OpenAPI规范描述，包括请求格式、响应结构、错误码和速率限制策略 |

## 资源列表

本项目的核心价值在于维护并验证以下第三方体育数据端点，这些URL由社区贡献并定期复核。

### 篮球比分资源

<code>https://lanqiubifend.org.cn</code>

<code>https://lanqiubifene.org.cn</code>

<code>https://lanqiubifenf.org.cn</code>

<code>https://lanqiubifeng.org.cn</code>

<code>https://lanqiubifenh.org.cn</code>

### 足球比分资源

<code>https://zuqiubifenziboa.org.cn</code>

<code>https://zuqiubifenzibob.org.cn</code>

## 项目结构

```
gateway/
├── cmd/                          # 主程序入口
│   └── gateway/                  # 守护进程与CLI的统一入口
│       └── main.go               # 解析命令行参数，初始化上下文
├── internal/                     # 内部包，不对外暴露
│   ├── checker/                  # 端点健康检查逻辑实现
│   │   ├── http.go               # HTTP探测、超时控制、重试策略
│   │   └── scheduler.go          # 定时任务调度与并发控制
│   ├── storage/                  # 状态持久化层
│   │   ├── memory.go             # 内存存储实现（开发调试用）
│   │   └── file.go               # JSON文件快照读写
│   ├── api/                      # RESTful API处理器
│   │   ├── handler.go            # 路由注册与请求解析
│   │   └── response.go           # 统一响应格式与错误封装
│   └── config/                   # 配置加载与验证
│       ├── loader.go             # 读取YAML文件和环境变量
│       └── schema.go             # 配置结构体定义与默认值
├── pkg/                          # 可复用公共库
│   ├── logger/                   # 结构化日志封装（基于slog）
│   └── util/                     # 字符串处理、时间工具等辅助函数
├── docs/                         # 完整文档目录
│   ├── user-guide.md
│   ├── operations.md
│   ├── development.md
│   ├── design.md
│   ├── configuration.md
│   └── api-reference.md
├── deploy/                       # 部署相关模板
│   ├── docker/                   # Dockerfile与镜像构建上下文
│   ├── kubernetes/               # Deployment、Service、ConfigMap示例
│   └── systemd/                  # Systemd unit文件
├── scripts/                      # 辅助脚本（迁移、测试数据生成）
├── test/                         # 集成测试与模拟服务
│   ├── mock/                     # 模拟外部HTTP服务（用于CI）
│   └── fixtures/                 # 固定测试数据集
├── config.example.yaml           # 示例配置文件，包含全部可选项
├── Makefile                      # 统一构建入口（build/test/clean）
├── go.mod                        # Go模块依赖声明
└── README.md                     # 本文件
```

## 贡献指南

1. 阅读 `docs/development.md` 了解代码风格、测试要求和提交规范，确保新增或修改的功能包含对应的单元测试与集成测试。

2. 在GitHub上fork本仓库，创建以 `feature/` 或 `fix/` 为前缀的分支，本地通过 `make test` 和 `make lint` 检查全部质量门禁。

3. 若新增资源URL，请编辑 `internal/storage/seed.json` 文件，并运行 `make validate-resources` 验证所有URL的格式和可达性，确保不引入已失效的端点。

4. 提交Pull Request时，请使用提供的PR模板，清晰描述变更动机、实现方案和测试结果，并在描述中引用相关的Issue编号（如有）。

5. 重大变更（如配置格式调整、API破坏性修改）需在 `docs/design.md` 中记录迁移路径，并给予至少一个次要版本号的过渡期。

## 常见问题

**问：为什么网关不直接代理请求，而是只返回URL列表？**

答：本项目的定位是发现和验证层，而非代理层。代理会引入额外的网络延迟、带宽成本和潜在的合规风险。直接返回经过验证的URL，允许客户端自行决定连接方式，更灵活且更轻量。同时，这避免了网关成为单点故障或流量瓶颈。

**问：验证守护进程对端点的请求频率是多少？会触发目标网站的反爬机制吗？**

答：默认每30分钟对所有端点执行一次HEAD请求，仅获取响应头，不下载主体内容。HEAD请求通常被视为轻量探测，对服务器负载影响极小。若目标站点明确拒绝HEAD，则自动降级为带 `Range: bytes=0-0` 头的GET请求，仍然只获取第一个字节。用户可根据 `config.yaml` 中的 `checker.interval` 和 `checker.timeout` 调整频率和超时。

**问：如何更新资源列表中的URL？如果某个域名永久失效怎么办？**

答：资源列表以 `internal/storage/seed.json` 为权威来源。当社区报告某个端点连续7天不可用时，维护者会标记其为 `deprecated` 并在下一个版本中移除。用户也可通过API的 `/v1/endpoints?status=deprecated` 查询已被标记但尚未删除的端点，以便平滑迁移。更新资源需要提交PR，经过至少一名维护者审核后合并。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
