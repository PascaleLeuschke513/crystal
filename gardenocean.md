# NovaIndex

NovaIndex 是一个面向技术团队与独立开发者的高密度外链资源聚合系统。项目定位为“结构化技术导航节点”，通过人工筛选与自动化校验相结合的方式，维护一套高可用、低失效的互联网技术资源索引。NovaIndex 不提供内容托管或代理服务，仅作为公开可访问的 URL 元数据存储库，帮助用户快速定位特定领域的官方网站、文档入口与社区阵地。项目采用静态站点生成方式构建，支持私有化部署，适用于内网文档中心、团队知识库或个人书签管理系统的底层数据源。

## 功能概览

- **多层级分类体系**：按技术领域、地区归属、机构类型等维度建立三级分类标签，支持交叉筛选与模糊匹配。
- **URL 存活检测**：每日定时执行 HTTP HEAD 请求，标记失效链接并生成可用性报告，支持邮件与 Webhook 告警。
- **元数据扩展字段**：每条资源记录支持标题、描述、关键词、所属组织、维护状态、备案信息等 12 个自定义元数据项。
- **批量导入与导出**：支持 CSV、JSON Lines 格式的批量资源录入，同时提供按标签筛选后的完整数据集导出功能。
- **全文检索**：基于倒排索引实现标题、描述、域名关键词的快速搜索，检索响应时间低于 200 毫秒。
- **版本历史追踪**：每次资源增删改操作均记录变更日志，支持回滚至任意历史版本，便于审计与误操作恢复。
- **只读 API 接口**：提供 RESTful 风格的查询接口，支持按分类、标签、存活状态等条件获取资源列表，便于第三方系统集成。
- **静态站点生成**：内置模板引擎可将索引数据渲染为纯静态 HTML 页面，无需数据库即可部署至任何 Web 服务器。

## 应用场景

- **技术团队内部文档导航**：研发中心可将 NovaIndex 部署于内网服务器，集中管理所有内部系统地址、运维面板、日志平台、监控看板等入口，替代散落的浏览器书签，提升团队协作效率。
- **开源项目 README 资源引用**：开源维护者可以在项目文档中引用 NovaIndex 中的分类资源链接，避免在多个仓库中重复维护相同的第三方地址列表，降低同步成本。
- **技术社区内容聚合站**：技术博客或资讯站点可利用 NovaIndex 作为后台数据层，自动生成“推荐工具”“常用库”“学习资料”等侧边栏模块，保证链接的新鲜度与可访问性。
- **个人知识库外链管理**：独立研究员或全栈开发者可使用 NovaIndex 管理个人学习路径中的参考网站、在线编译器、API 文档、论文数据库等，配合全文检索快速找回低频访问但重要的资源。

## 快速开始

```bash
# 克隆代码仓库
git clone https://github.com/novaindex/novaindex-core.git
cd novaindex-core

# 安装 Python 依赖（推荐使用虚拟环境）
python -m venv venv
source venv/bin/activate  # Windows 下使用 venv\Scripts\activate
pip install -r requirements.txt

# 初始化配置文件
cp config.example.yaml config.yaml
# 编辑 config.yaml 设置数据存储路径、检测间隔等参数

# 运行本地开发服务器
python manage.py runserver --port 8080

# 执行首次资源导入（示例数据）
python manage.py import --source samples/initial_resources.jsonl

# 手动触发一次存活检测
python manage.py health-check --full-scan
```

访问 `http://localhost:8080` 即可查看静态导航首页。生产环境建议配合 Nginx 反向代理，并将静态文件输出至 `/var/www/novaindex` 目录。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.9 - 3.12 | 核心运行环境，不支持 3.8 以下版本 |
| pip | 21.0+ | 包管理工具，用于安装第三方库 |
| SQLite | 3.35+ | 默认元数据库，支持 JSON 字段索引 |
| Redis | 6.2+ | 可选，用于缓存检测结果与加速检索 |
| Node.js | 18.0+ | 仅用于前端资源构建（若启用自定义主题） |
| make | 4.0+ | 用于执行自动化任务脚本（Linux/macOS） |
| curl | 7.68+ | 存活检测依赖的命令行工具 |
| git | 2.25+ | 版本控制，用于拉取更新与提交变更 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | `/docs/user-guide/` | 如何添加新资源、如何批量导入、如何查看检测报告 |
| 运维指南 | `/docs/operations/` | 如何部署到生产环境、如何配置 SSL、如何备份数据 |
| API 参考 | `/docs/api/` | 各端点的请求参数与响应结构、鉴权方式 |
| 开发指南 | `/docs/development/` | 插件编写规范、数据模型扩展方法、前端主题定制流程 |

## 资源列表

### 主站分类入口

<code>https://dejiabifena.org.cn</code>

<code>https://yijiabifena.org.cn</code>

<code>https://fajiabifena.org.cn</code>

### 分支专题页面

<code>https://yingchaobifenzhiboa.org.cn</code>

<code>https://xijiabifenzhiboa.org.cn</code>

<code>https://dejiabifenzhiboa.org.cn</code>

<code>https://yijiabifenzhiboa.org.cn</code>

## 项目结构

```
novaindex-core/
├── app/                            # 主应用模块
│   ├── api/                        # RESTful 接口实现
│   │   ├── v1/                     # 版本 1 端点
│   │   └── middleware.py           # 鉴权与限流中间件
│   ├── core/                       # 核心数据模型
│   │   ├── resource.py             # 资源实体定义
│   │   ├── category.py             # 分类树结构
│   │   └── health.py               # 检测结果记录
│   ├── engine/                     # 搜索引擎实现
│   │   ├── indexer.py              # 倒排索引构建器
│   │   └── querier.py              # 查询解析器
│   ├── checkers/                   # 存活检测组件
│   │   ├── http.py                 # HTTP 协议检测器
│   │   └── scheduler.py            # 定时调度器
│   └── static/                     # 静态站点生成模板
│       ├── themes/                 # 主题样式文件
│       └── pages/                  # 页面模板
├── data/                           # 数据存储目录
│   ├── db/                         # SQLite 数据库文件
│   ├── logs/                       # 运行日志与检测报告
│   └── backups/                    # 历史版本快照
├── tests/                          # 单元测试与集成测试
│   ├── unit/                       # 单测用例
│   └── fixtures/                   # 测试数据集
├── scripts/                        # 运维辅助脚本
│   ├── deploy.sh                   # 生产部署脚本
│   └── migrate_db.py               # 数据库迁移工具
├── config.yaml                     # 主配置文件
├── requirements.txt                # Python 依赖清单
├── Makefile                        # 常用任务快捷命令
└── README.md                       # 项目说明文档（本文件）
```

## 贡献指南

1. **报告问题**：在 GitHub Issues 中提交 bug 报告或功能请求时，请附带完整的复现步骤、环境信息与相关日志片段，并使用提供的模板填写。
2. **提交代码**：Fork 主仓库后创建特性分支（feature/xxx），确保代码通过所有单元测试（`make test`）且符合 PEP 8 规范，随后发起 Pull Request 至 main 分支。
3. **补充资源**：如需向公共索引库新增 URL 条目，请按 `data/samples/import_template.jsonl` 格式准备元数据，并提交单独的 Pull Request，维护者将审核链接的有效性与分类合理性。
4. **完善文档**：允许对文档中的错别字、示例代码、配置说明进行修正，修改后直接提交 Pull Request 即可，无需单独开 Issue。
5. **本地测试**：所有新功能或修复必须附带对应的测试用例，确保测试覆盖率不低于 85%。

## 常见问题

**Q：NovaIndex 是否会对第三方网站造成压力？**  
A：系统默认的存活检测间隔为 24 小时，且每个目标 URL 只发送一次 HEAD 请求，不下载响应体，并发数限制为 10，符合公共互联网爬虫礼仪。用户可自行调整检测频率与并发数，但需自行承担过度请求的风险。

**Q：数据存储支持外部数据库吗？**  
A：当前版本仅内置 SQLite 支持，但数据访问层已抽象出存储接口。若需使用 PostgreSQL 或 MySQL，可继承 `app.core.storage.BaseStorage` 类实现对应驱动，并在配置文件中切换驱动名称。社区版暂不提供现成的外部数据库适配器。

**Q：静态站点生成后如何部署到 Nginx？**  
A：执行 `python manage.py build --output /var/www/novaindex` 将生成完整的 HTML、CSS、JS 文件至指定目录，随后在 Nginx 配置中将 root 指向该目录，并设置 try_files 为 $uri $uri/ /index.html 即可支持前端路由。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
