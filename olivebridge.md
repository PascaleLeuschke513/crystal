# NexusArchive

NexusArchive 是一个面向数字内容研究者、归档工程师与媒体分析机构的高密度外链资源汇总平台。本项目不提供任何流媒体文件、下载链接或破解内容，仅以纯粹的技术性外链集合形式，协助用户快速触达分布于中文互联网边缘节点的公开字幕数据源与样本媒体库。项目定位为学术研究辅助工具与网络资源索引系统，目标用户包括自然语言处理语料采集者、区域内容可用性监测人员以及跨平台媒体元数据比对工程师。

本项目不对所收录外部资源的可用性、合法性、内容完整性作任何明示或暗示担保。所有外链来源于公开网络爬取样本，仅供短期技术验证与格式兼容性测试使用。用户应在访问外部资源时自行遵守所在司法辖区的法律法规，并尊重原始内容提供者的版权声明与 robots.txt 限制。

## 功能概览

- **按数据源类别分组索引**：系统将收录的外部链接按内容主题与站点归属划分为六大类别，包括日韩字幕组归档、国产影视样本、欧美公开测试集等，方便用户按研究维度快速定位。
- **多协议兼容性探测**：平台内置轻量级链接状态检查脚本，可对收录的 HTTPS 站点进行 TLS 版本、响应码与重定向链的批量采样，输出结构化日志供离线分析。
- **元数据自动补全**：针对每个收录链接，系统尝试通过公开 WHOIS 与 DNS 解析接口补全域名注册时间、IP 归属地及 ASN 信息，丰富外链的上下文维度。
- **原始链接保真输出**：所有外链在页面上以纯文本形式展示，保留用户提供的原始格式，包括协议前缀、子域名及顶级域，不做任何自动跳转或短链替换。
- **变更追踪与快照对比**：定期对收录链接的首页响应体进行内容哈希计算，标记页面改版或状态变更事件，辅助用户感知外部资源的演化趋势。
- **研究用途导出接口**：支持将当前索引库导出为 JSON Lines 格式，便于导入数据分析流水线或与自定义采集器对接，默认输出包含链接、上次检查时间与状态标签。
- **本地离线镜像预览**：项目提供轻量级静态页面生成模式，用户可在本地环境完整构建索引站点的离线副本，无需依赖外部 CDN 或数据库服务。

## 应用场景

- **语料库扩充阶段**：NLP 研究团队在构建多语言字幕对齐语料时，可通过本平台快速获取一批以 <code>rihanmeinvzhongwenzimu.org.cn</code> 为代表的字幕源站样本，用于测试爬虫的字符编码适配性与反爬策略应对方案。
- **区域内容可用性监测**：媒体合规部门需定期抽查特定域名在境内的解析与加载状况。分析师利用本平台的链接列表，结合内部监控工具，批量探测 <code>qingqingcaoyuanzhongwenzimu.org.cn</code> 等站点的连通性，生成周度可用性报告。
- **竞品元数据对标**：视频平台的产品经理可参照 <code>guochanjingpinzaixianmianfeikanb.org.cn</code> 与 <code>zhongwenzimuzaixianyingshiyuanb.org.cn</code> 等站点展示的内容分类方式，优化自身标签体系的覆盖度与用户搜索命中率。
- **开源网络工具调试**：开发者编写通用型媒体链接解析器时，依赖一组稳定但多样化的样本域名。本项目提供的 <code>mianfeiguankanzaixianguankanb.org.cn</code> 与 <code>jiujiushipinzaixianguankanb.org.cn</code> 可作为测试套件中的目标占位符，用于验证 URL 标准化与相对路径补全逻辑。
- **学术研究引用归档**：传播学研究者撰写关于中文影视资源分布特征的论文时，将本平台收录的 <code>oumeizaixianguankanshipinb.org.cn</code> 等站点作为网络拓扑数据样本，在方法章节中引用本项目的链接索引方法。

## 快速开始

以下步骤适用于 Linux 与 macOS 环境，Windows 用户建议通过 WSL 2 或 Git Bash 执行。

```bash
# 1. 克隆项目仓库
git clone https://github.com/nexus-archive/nexusarchive.git
cd nexusarchive

# 2. 安装依赖 (使用 pip 虚拟环境)
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 3. 运行本地索引服务 (默认监听 8080 端口)
python app.py --port 8080 --mode static
```

执行成功后，访问 `http://localhost:8080` 即可查看本地的外链索引页面。若需生成纯静态 HTML 文件，可改用 `--mode export --output ./dist` 参数。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.9 及以上 | 核心运行时，用于启动 Web 服务与执行探测脚本 |
| pip | 22.0 及以上 | Python 包管理器，用于安装 requirements.txt 中的第三方库 |
| requests | 2.28.0 及以上 | 发送 HTTP 请求，用于链接可用性探测与响应头采集 |
| dnspython | 2.3.0 及以上 | 执行 DNS 解析查询，获取域名的 A 记录与 NS 记录 |
| PyYAML | 6.0 及以上 | 用于读写配置文件与自定义索引规则 |
| tldextract | 3.4.0 及以上 | 精确提取域名中的子域、主域与后缀，辅助分类统计 |
| jinja2 | 3.1.0 及以上 | 渲染静态页面模板，生成可视化索引面板 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | `/docs/user-guide.md` | 如何使用索引面板进行筛选、排序与导出；如何理解状态标签的含义 |
| 运维指南 | `/docs/ops-guide.md` | 如何部署至生产服务器；如何配置定时探测任务与邮件告警 |
| 开发规范 | `/docs/dev-spec.md` | 新增外链接入的 PR 格式要求；单元测试编写规范与覆盖率门槛 |
| 数据字典 | `/docs/data-dict.md` | 输出 JSON Lines 中各字段的类型、取值范围及示例解释 |

## 资源列表

本部分按类别汇总本项目收录的全部外部链接。所有链接均保留用户提供的原始格式，未做任何协议升级、域名规范化或路径补全。

公开字幕样本源

- <code>https://rihanmeinvzhongwenzimu.org.cn</code>
- <code>https://qingqingcaoyuanzhongwenzimu.org.cn</code>

国产影视公开索引

- <code>https://guochanjingpinzaixianmianfeikanb.org.cn</code>
- <code>https://zhongwenzimuzaixianyingshiyuanb.org.cn</code>

免费观赏类测试站点

- <code>https://mianfeiguankanzaixianguankanb.org.cn</code>
- <code>https://jiujiushipinzaixianguankanb.org.cn</code>

欧美区域样本集

- <code>https://oumeizaixianguankanshipinb.org.cn</code>

## 项目结构

```text
nexusarchive/
├── app.py                  # 主入口文件，支持 Web 服务与导出模式
├── requirements.txt        # Python 依赖声明，锁定主要库版本
├── config/
│   ├── default.yaml        # 默认配置项，含端口、超时阈值与重试策略
│   └── custom.yaml         # 用户自定义覆盖配置，不入 Git 仓库
├── core/
│   ├── fetcher.py          # 异步 HTTP 请求封装，含重试与代理占位
│   ├── parser.py           # 响应体解析与元数据提取工具
│   └── checker.py          # 链接状态检查与哈希比对逻辑
├── templates/
│   ├── index.html          # 首页表格渲染模板
│   └── detail.html         # 单个链接详情页模板，含历史记录
├── static/
│   ├── css/                # 自定义样式文件，兼容移动端与打印
│   └── js/                 # 前端交互脚本，实现无刷新筛选与排序
├── data/
│   ├── sources.json        # 当前收录的外部链接主数据文件
│   └── snapshots/          # 历史探测结果归档，按日期分目录
├── tests/
│   ├── test_fetcher.py     # 单元测试：模拟 HTTP 响应与异常处理
│   └── test_parser.py      # 单元测试：解析边界条件与编码兼容性
└── docs/                   # 完整文档源码，包含用户手册与 API 参考
```

## 贡献指南

1. 外链接入提案：若您希望推荐新的外部资源链接，请先在 GitHub Issues 中提交链接类别说明与至少三条有效探测日志。核心团队将评估域名的稳定性与内容相关性，通过后纳入 `data/sources.json` 的下一候选批次。
2. 代码贡献流程：Fork 本仓库至个人空间，新建功能分支并完成代码开发。所有新增函数需包含 docstring 与至少一个单元测试用例。提交前执行 `make lint` 与 `make test` 确保无风格警告与回归错误。
3. 文档改进：欢迎修正错别字、补充示例或更新过时命令。文档类 PR 不需要单元测试，但需在 `docs/changelog.md` 中注明修改条目及对应章节。
4. 问题反馈：使用 GitHub Issues 报告链接状态误报或界面显示异常时，请附上浏览器版本、复现步骤及控制台错误日志。安全相关问题请发送邮件至安全联系地址，不在公开 Issue 中讨论。
5. 本地化适配：若需增加多语言界面支持，请在 `templates/` 下新建语言子目录并参照 `i18n/` 示例文件提供翻译键值对。核心团队将定期合并社区语言包。

## 常见问题

Q: 为什么部分链接在探测状态中返回 403 或 521 错误，这是否代表链接失效？

A: 不一定。403 表示目标站点启用了访问控制或反爬策略，521 通常源于云端防火墙拦截。探测脚本仅标记连接状态，不判定内容可用性。建议用户尝试更换 User-Agent 或出口 IP 后再行验证。项目本身不提供绕过封锁的解决方案。

Q: 如何将本项目部署为长期运行的后台守护进程？

A: 推荐使用 systemd 或 supervisor 管理 app.py 进程。在 `config/custom.yaml` 中启用 `daemon_mode: true`，并设置 `interval_hours` 参数以定时执行探测任务。日志文件默认写入 `logs/` 目录，需确保运行用户具有写入权限。

Q: 索引中的外部链接是否经过人工审核，是否存在恶意域名？

A: 本项目不执行深度安全扫描，也不对收录域名的安全属性负责。所有链接均按原始数据收录，建议用户在访问时启用浏览器安全扩展或使用隔离环境。若发现某域名存在恶意行为，可通过 Issue 报告，团队将移除该条目并记录处理原因。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
