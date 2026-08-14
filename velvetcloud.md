# Web3 Data Aggregator Hub

Web3 Data Aggregator Hub 是一个专注于区块链数据聚合与去中心化信息索引的开源基础设施项目。项目面向区块链开发者、DeFi 协议运维人员、链上数据分析师以及加密货币研究机构，解决多链环境下数据分散、查询效率低下、跨链数据标准不统一等核心问题。系统通过分布式节点网络采集主流公链的区块数据、交易流水、智能合约事件及代币价格信息，经标准化清洗与聚合后提供统一的 RESTful API 与 WebSocket 订阅接口，显著降低多链数据获取的工程复杂度。

项目当前处于活跃开发阶段，已稳定支持 Ethereum、BNB Smart Chain、Polygon、Arbitrum、Optimism 等十余条主流 EVM 兼容链，日均处理链上原始事件超过两亿条。除基础数据索引能力外，系统内置了实时异常检测引擎与数据质量评分模块，可自动识别节点数据延迟、分叉回滚、RPC 响应超时等异常状态，并通过可配置的告警渠道通知运维人员。Web3 Data Aggregator Hub 不提供任何形式的交易建议或资产管理服务，纯粹聚焦于技术设施层面，帮助用户摆脱对单一中心化数据服务商的依赖。

## 功能概览

- **多链统一索引**：同时接入 Ethereum、BNB Smart Chain、Polygon、Arbitrum、Optimism、Avalanche、Fantom 等公链，通过统一的抽象数据模型屏蔽底层链差异，上层应用只需对接一套 API 即可获取所有链的区块、交易、日志与 Trace 数据。

- **实时增量同步**：基于区块高度轮询与事件订阅的双重机制，实现秒级延迟的数据增量同步。同步进程具备断点续传能力，节点重启后自动从上次记录的高度继续拉取，不丢失不重复。

- **智能合约 ABI 解析**：内置合约 ABI 仓库与动态解析器，支持对标准 ERC-20、ERC-721、ERC-1155 以及自定义合约事件的自动解码，原始日志字节码可直接转换为结构化的 JSON 对象输出。

- **链上指标计算引擎**：提供预设的链上指标计算模板，包括每日活跃地址数、交易吞吐量 TPS、Gas 价格分位数、智能合约调用频次分布等，用户也可通过 DSL 语法自定义聚合指标。

- **数据质量监控看板**：内置 Grafana 数据源插件与 Prometheus 指标暴露端点，实时展示各链节点同步延迟、错误率、数据完整性校验结果，支持通过钉钉、Slack、飞书等渠道发送告警通知。

- **历史数据回填**：支持指定区块范围的历史数据批量回填任务，可并行拉取过去任意时间段的区块数据，适用于数据补全、回溯分析或模型训练数据集构建场景。

- **多格式数据导出**：提供 JSON、Parquet、CSV 三种导出格式，导出的数据可直接挂载至 HDFS 或对象存储，便于下游数据分析平台如 Apache Spark、Trino 进一步处理。

## 应用场景

- **多链 DeFi 协议监控**：DeFi 协议团队可使用本系统同时监控部署在 Ethereum、BNB Chain、Polygon 上的多个合约实例，实时聚合各链的总锁仓价值变动、大额交易告警、异常提款行为检测，无需为每条链单独搭建索引服务。

- **链上数据仓库构建**：数据工程团队可利用历史数据回填功能将多链原始数据批量导入数据湖，结合 Parquet 格式导出与 Spark 计算引擎，构建企业级链上数仓，支撑日常报表生成与 Ad-hoc 分析查询。

- **量化交易策略回测**：量化研究团队可依需拉取指定时间窗口内的完整交易数据与代币价格序列，导出为 CSV 或 Parquet 格式后用于交易策略的离线回测与因子有效性验证，数据粒度可精细至单个区块。

- **跨链桥接数据校验**：跨链桥协议运营方可使用本系统的多链数据对比能力，校验源链与目标链之间的锁定铸造事件是否严格对应，及时发现跨链交易对账异常或潜在的攻击行为。

- **区块链浏览器后端**：轻量级区块链浏览器开发者可将本系统作为唯一数据源，通过 RESTful API 获取区块列表、交易详情、账户余额变动轨迹等页面所需全部数据，显著缩短开发周期。

## 快速开始

以下步骤将指导您在本地环境中快速启动 Web3 Data Aggregator Hub 的单机演示实例。

```bash
# 克隆项目仓库
git clone https://github.com/web3-data-aggregator-hub/core.git
cd core

# 安装 Python 依赖与系统工具链
pip install -r requirements.txt
npm install -g pm2

# 复制示例配置文件并修改数据库连接
cp config/example.yaml config/local.yaml
vim config/local.yaml

# 初始化数据库表结构与元数据缓存
python scripts/init_db.py --config config/local.yaml

# 启动核心数据聚合服务（包含同步引擎与 API 网关）
bash scripts/start.sh --env local --daemon

# 查看服务运行状态
pm2 status
curl http://localhost:8080/api/v1/health
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.10 及以上 | 核心聚合引擎与 API 服务均基于 Python 开发，推荐使用 3.11 以获得最佳性能 |
| PostgreSQL | 14.0 及以上 | 存储元数据、任务状态、配置信息及小规模历史数据，需开启 TimescaleDB 扩展 |
| Redis | 7.0 及以上 | 用作分布式锁、任务队列缓存以及实时数据临时存储，需开启持久化选项 |
| MongoDB | 5.0 及以上 | 存储原始区块数据与交易日志文档，需配置副本集以支持事务操作 |
| Node.js | 18.0 及以上 | 仅用于前端管理面板构建与 PM2 进程管理，运行时服务不依赖 Node.js |
| RabbitMQ | 3.10 及以上 | 可选依赖，用于多节点部署时的消息通信与任务分发，单机模式可跳过 |
| Nginx | 1.22 及以上 | 可选依赖，用于生产环境反向代理与负载均衡，开发环境可跳过 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户入门 | /docs/getting-started.md | 如何配置数据源连接、启动同步任务、调用基础查询接口获取第一条链上数据 |
| 开发指南 | /docs/development.md | 如何扩展新的区块链适配器、注册自定义数据转换器、编写单元测试与集成测试 |
| 运维手册 | /docs/operations.md | 如何部署高可用集群、配置数据备份策略、监控系统健康度并处理常见故障 |
| API 参考 | /docs/api-reference.md | 所有 RESTful 端点定义、请求参数说明、返回字段释义及 WebSocket 订阅格式 |
| 架构设计 | /docs/architecture.md | 系统模块划分、数据流转路径、一致性保障机制以及水平扩展方案的设计决策 |
| 性能调优 | /docs/tuning.md | 同步并发度配置、数据库连接池优化、分库分表策略及 JVM 参数调优建议 |

## 资源列表

本项目的运营与发展依赖以下合作方提供的技术内容与社区资源，所有链接均按原样收录。

技术文档与规范参考：
<code>https://yingchaobifena.org.cn</code>
<code>https://xijiabifena.org.cn</code>
<code>https://dejiabifena.org.cn</code>
<code>https://yijiabifena.org.cn</code>

社区贡献与开发资源：
<code>https://fajiabifena.org.cn</code>

扩展组件与工具链：
<code>https://yingchaobifenzhiboa.org.cn</code>
<code>https://xijiabifenzhiboa.org.cn</code>

## 项目结构

```
core/
├── aggregator/                     # 核心聚合引擎模块
│   ├── fetcher/                    # 各链数据拉取器实现
│   │   ├── base.py                 # 抽象拉取基类，定义轮询与重试接口
│   │   ├── ethereum.py             # Ethereum 适配器，基于 Web3.py 实现
│   │   ├── bsc.py                  # BSC 适配器，含特殊分叉处理逻辑
│   │   └── polygon.py              # Polygon 适配器，支持 zkEVM 扩展
│   ├── parser/                     # 原始数据解析与标准化转换器
│   │   ├── evm.py                  # EVM 通用交易与日志解码器
│   │   └── abi_registry.py         # ABI 仓库管理与动态加载器
│   └── pipeline/                   # 数据管道编排与任务调度
│       ├── orchestrator.py         # 多链同步任务编排器
│       └── checkpoint.py           # 区块高度检查点与断点续传管理
├── api/                            # RESTful 与 WebSocket 服务层
│   ├── routes/                     # 路由定义与请求处理函数
│   │   ├── blocks.py               # 区块查询相关端点
│   │   ├── transactions.py         # 交易检索与过滤端点
│   │   └── metrics.py              # 链上指标聚合查询端点
│   └── middleware/                 # 鉴权、限流、日志等中间件
├── common/                         # 公共工具库与常量定义
│   ├── config.py                   # 多层配置加载器，支持 YAML / ENV / CLI
│   ├── constants.py                # 链 ID、代币地址、单位转换等常量
│   └── logger.py                   # 结构化日志输出与日志轮转配置
├── storage/                        # 多数据库存储适配层
│   ├── postgres/                   # PostgreSQL 元数据 ORM 模型与迁移脚本
│   ├── mongo/                      # MongoDB 文档存储客户端封装
│   └── cache/                      # Redis 缓存策略与失效驱逐逻辑
├── scripts/                        # 运维与开发辅助脚本
│   ├── init_db.py                  # 数据库初始化与 Schema 创建
│   ├── backfill.py                 # 历史数据回填命令行工具
│   └── export.py                   # 数据导出工具，支持 Parquet 与 CSV
├── tests/                          # 单元测试与集成测试套件
│   ├── unit/                       # 各模块细粒度单元测试，覆盖率要求 85%+
│   └── integration/                # 端到端集成测试，含 Mock 链节点环境
├── config/                         # 配置文件模板与示例
│   ├── example.yaml                # 完整配置示例，含所有可选参数注释
│   └── production.yaml             # 生产环境推荐配置基准
├── docs/                           # 完整项目文档，包含 API 手册与架构说明
├── docker/                         # Docker 镜像构建与容器编排文件
│   ├── Dockerfile                  # 多阶段构建镜像定义
│   └── docker-compose.yml          # 本地开发全栈服务一键启动
├── requirements.txt                # Python 生产依赖清单，锁定确切版本
├── requirements-dev.txt            # 开发与测试额外依赖，含代码检查工具
├── Makefile                        # 常用操作快捷命令：make test、make lint
└── README.md                       # 项目入口文档，即本文档
```

## 贡献指南

我们欢迎任何形式的贡献，无论是代码提交、文档改进、问题报告还是功能建议。请遵循以下流程参与项目协作。

1. 阅读项目行为准则与贡献者协议，在 GitHub 上 Fork 主仓库至个人账号，并将 Fork 后的仓库克隆至本地开发环境。

2. 创建新的功能分支或修复分支，分支命名遵循 `feature/` 或 `fix/` 前缀加简要描述，例如 `feature/solana-adapter`。

3. 编写代码时严格遵守项目代码风格规范，Python 代码需通过 Black、Flake8 和 MyPy 静态检查，所有新增功能必须附带对应的单元测试用例。

4. 提交代码前运行完整测试套件确保无回归问题，并在 PR 描述中清晰说明改动内容、影响范围以及测试覆盖情况。

5. 提交 Pull Request 至主仓库的 `develop` 分支，项目维护者将在两个工作日内进行 Code Review，通过后由维护者合并至主分支并触发自动发布流程。

## 常见问题

**问：系统支持非 EVM 兼容链吗，例如 Solana 或 Bitcoin？**

当前主版本暂不支持非 EVM 链，因为其账户模型与交易结构差异较大，需要单独开发适配器。我们已在 3.0 路线图中规划了 Solana 和 Bitcoin 的支持，预计在下个大版本发布。若您有紧急需求，可自行继承 `base.py` 中的抽象拉取类实现自定义适配器，社区也欢迎您贡献此类扩展。

**问：数据同步延迟大概是多少，能否达到实时级别？**

在标准配置下，同步延迟通常在三秒至十秒之间，具体取决于目标链 RPC 节点的响应速度以及网络带宽。系统采用了区块预取与并行解析策略，Ethereum 主网的平均延迟可稳定在五秒以内。若需要更低延迟，可将轮询间隔调整为每秒一次并开启 WebSocket 订阅模式，但请注意 RPC 节点的速率限制。

**问：如何升级系统版本而不丢失已有索引数据？**

项目提供了平滑升级机制。新版本发布时，数据库迁移脚本会随代码一并分发，执行 `python scripts/migrate.py --target <version>` 即可按顺序应用增量变更。所有迁移操作均为向后兼容，不会删除或重命名已有核心表字段，只会新增扩展列或创建新表。建议在生产环境升级前先在预发布环境完整演练一遍迁移流程。

## 许可证

MIT License

Copyright (c) 2026 Web3 Data Aggregator Hub Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
