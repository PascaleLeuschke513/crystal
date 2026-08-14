# ResourceBridge

一个面向技术内容创作者与开发者的高质量外链资源聚合与导航系统。ResourceBridge 定位于解决开发者在查阅技术文档、学习新框架、寻找在线工具时面临的多平台信息分散、检索效率低下的问题，通过人工筛选与社区共建的方式，构建一个结构清晰、访问稳定的技术资源目录树。项目本身不存储或托管任何第三方内容，仅提供客观的 URL 索引与分类导航服务，适用于个人开发者、技术团队内部知识库以及开源社区文档的扩展引用场景。

## 功能概览

- **分类资源目录**：按内容类型、领域、适用人群对收录链接进行多级标签划分，支持快速过滤。
- **实时可用性检测**：后台定时对收录的 URL 执行可达性检查，并在前端标注异常状态。
- **简洁只读视图**：所有资源以纯文本列表形式呈现，无冗余广告或推荐算法干扰。
- **社区提交入口**：允许用户通过 Pull Request 或 Issue 模板提交新的资源链接，经审核后合并。
- **本地化缓存镜像**：为高频访问的文档类站点提供静态文本缓存，提升国内访问速度。
- **全文检索支持**：基于标题、描述、标签及 URL 关键字的轻量级搜索接口。
- **开放数据导出**：支持将整个资源目录导出为 JSON 或 CSV 格式，便于二次开发。

## 应用场景

1. 技术团队内部知识库搭建：团队 Leader 可将 ResourceBridge 作为默认的导航页，统一成员查阅外部文档的入口，减少因使用不同搜索引擎导致的信息差异。
2. 开源项目文档引用：开源项目维护者可在自己的 README 或 Wiki 中引用 ResourceBridge 中的分类链接，替代冗长的外链列表，提升文档可读性。
3. 技术博客创作辅助：技术博主在撰写教程时，可通过 ResourceBridge 快速获取相关主题的官方文档、社区讨论及视频教程地址，节省检索时间。
4. 线上编程训练营教学：培训机构可将 ResourceBridge 配置为学员的实验环境起始页，确保所有学员访问到统一版本的外部学习资源。
5. 个人开发者收藏夹管理：开发者可使用 ResourceBridge 替代浏览器书签，实现跨设备、跨浏览器的资源同步与分类整理。

## 快速开始

以下步骤适用于在本地开发环境中部署 ResourceBridge 导航服务。

```bash
# 克隆项目仓库
git clone https://github.com/resourcebridge/resourcebridge.git

# 进入项目目录
cd resourcebridge

# 安装依赖（基于 Node.js 22 LTS 与 pnpm）
pnpm install

# 复制环境变量模板并填写本地配置
cp .env.example .env

# 启动开发服务器（默认监听端口 3000）
pnpm run dev
```

访问控制台输出的本地地址（通常为 http://localhost:3000）即可查看导航主页。生产环境部署请参考 `docs/deployment.md` 中的 Nginx 反向代理配置与 PM2 持久化方案。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Node.js | 22.x LTS | 运行时环境，推荐使用 nvm 管理版本 |
| pnpm | 9.x 或以上 | 包管理器，用于依赖安装与工作区管理 |
| PostgreSQL | 16.x | 主数据库，存储资源元数据与用户提交记录 |
| Redis | 7.x | 缓存层，用于 URL 可用性检测结果与搜索热词 |
| Nginx | 1.24 或以上 | 生产环境反向代理与静态资源压缩（可选） |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|-----|------|----------|
| 用户手册 | `docs/user-guide/` | 如何使用导航页面、如何提交新资源、如何导出数据 |
| 管理员手册 | `docs/admin-guide/` | 如何审核提交、如何管理标签、如何查看检测日志 |
| 开发者指南 | `docs/developer-guide/` | 项目架构说明、API 接口文档、本地二次开发流程 |
| 部署运维 | `docs/deployment/` | 生产环境 Docker 部署、Kubernetes 编排、监控告警配置 |

## 资源列表

本导航项目当前收录以下外部资源链接，按类别分组展示。所有链接均来自用户原始数据，未经任何修改。

**直播与视频类**

- <code>https://wanghongzhibozaixianshipin.org.cn</code>
- <code>https://wanghongfulizhibo.org.cn</code>
- <code>https://guochanwanghongzhibozhuzaixian.org.cn</code>
- <code>https://guochanwanghongshipinzhibo.org.cn</code>
- <code>https://wanghongzhibomianfeiguankan.org.cn</code>
- <code>https://meinvzhibozaixiankan.org.cn</code>
- <code>https://guochanwanghongfulishipin.org.cn</code>

## 项目结构

```
resourcebridge/
├── apps/
│   ├── web/                         # 主站点 Next.js 应用
│   │   ├── src/
│   │   │   ├── app/                 # App Router 页面路由
│   │   │   ├── components/          # 可复用 UI 组件
│   │   │   ├── lib/                 # 数据获取与工具函数
│   │   │   └── styles/              # 全局样式与主题变量
│   │   └── public/                  # 静态资源（favicon、logo 等）
│   └── crawler/                     # 独立爬虫服务（Node.js）
│       ├── src/
│       │   ├── checker/             # URL 可用性检测模块
│       │   ├── parser/              # 页面标题与描述提取
│       │   └── scheduler/           # 定时任务调度
│       └── config/                  # 爬虫配置文件
├── packages/
│   ├── database/                    # Prisma ORM 模型与迁移
│   ├── types/                       # 共享 TypeScript 类型定义
│   └── utils/                       # 通用工具函数（日志、加密等）
├── docs/                            # 完整项目文档（见文档导航）
├── scripts/                         # 开发与部署辅助脚本
├── .env.example                     # 环境变量模板
├── docker-compose.yml              # 本地开发数据库容器编排
├── package.json                     # 根目录工作区配置
└── README.md                        # 本文件
```

## 贡献指南

我们欢迎社区提交新的资源链接、改进现有分类或修复代码缺陷。请遵循以下步骤：

1. 阅读 `docs/developer-guide/CONTRIBUTING.md` 了解整体贡献流程与行为准则。
2. 在 Issue 列表中搜索是否已有相同或类似的提议，避免重复工作。
3. Fork 本仓库，在您的分支上完成修改。对于新增资源链接，请使用 `scripts/submit-resource.ts` 脚本生成标准化 JSON 条目。
4. 提交 Pull Request 前，请确保本地通过所有单元测试（`pnpm test`）以及代码风格检查（`pnpm lint`）。
5. PR 描述中需清晰说明变更内容，并关联相关 Issue 编号。维护者将在 7 个工作日内完成审核。

## 常见问题

**Q: 为什么某些链接在导航中显示为不可用状态？**
A: 系统每 24 小时对收录链接执行一次 HTTP 头检测。如果目标站点返回 4xx 或 5xx 状态码，或超时超过 10 秒，则标记为不可用。该状态仅反映最近一次检测结果，不代表站点永久离线。

**Q: 我提交的资源链接多久会被收录？**
A: 社区提交的链接将在每周三和周六进行批量审核。审核标准包括：链接内容的稳定性、与现有分类的匹配度、以及是否存在重复。审核通过后会在下一次部署中生效，通常不超过 7 天。

**Q: 能否在私有网络环境中部署 ResourceBridge 并使用内网资源？**
A: 可以。您只需在 `.env` 文件中修改 `ALLOWED_DOMAINS` 变量，添加您的内网域名或 IP 段。同时需要关闭外部 URL 检测功能（设置 `ENABLE_CHECKER=false`），以避免内网地址被公网探针扫描。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:08:34
