# HyperLink Navigator

HyperLink Navigator 是一个面向技术内容创作者、开源文档维护者及本地化翻译团队的高效外链资源整理与导航系统。该项目并非传统意义上的爬虫或采集工具，而是一套基于静态标记语言与轻量级脚本引擎的链接资产管理方案。其核心目标在于解决多源、多批次、多语言外链资源在收录、校验、分类及呈现过程中的混乱问题，帮助用户从繁杂的 URL 文本中快速构建出结构清晰、可维护、可追溯的导航页面或文档附录。

目标用户包括：需要批量处理外部参考链接的技术文档工程师、运营多语言内容站点的本地化团队、以及希望为开源项目建立规范化外链引用体系的维护者。HyperLink Navigator 不依赖任何外部数据库或云服务，所有数据均以纯文本形式存储在项目仓库中，确保完全的可移植性与版本控制兼容性。

## 功能概览

- **批量链接收容与去重**：支持从纯文本、CSV 或简易标记列表中导入大量 URL，自动识别协议头与域名变体，剔除重复项并标记可疑短链。

- **分类标签与权重标记**：允许用户为每个链接赋予分类标签（如“文档”、“视频源”、“工具站”）及优先级权重，便于后续按主题或重要程度进行筛选排序。

- **结构树生成器**：根据链接所属分类或来源批次，自动生成 ASCII 目录树或 Markdown 嵌套列表，可用于直接粘贴至项目 README 或 Wiki 页面。

- **可用性嗅探（离线模式）**：提供可选的本地网络探测脚本，通过发送轻量级 HEAD 请求检查链接可达性，并输出超时或错误状态报告，辅助清理失效资源。

- **多格式导出器**：支持将整理后的链接库导出为 Markdown 表格、HTML 无序列表、JSON 结构体或纯文本索引，适应不同的文档发布环境。

- **批次追溯与注释绑定**：每个链接条目可绑定批次编号（如第 11/130 批）、录入时间及简短注释，方便追溯资源来源与更新历史。

## 应用场景

- **技术文档附录整理**：当编写大型开源项目的参考文档时，维护者需要引用数十个外部规范、教程或 API 参考站点。HyperLink Navigator 可将这些散落的链接统一收编，并按章节自动生成带注释的参考文献列表，避免手动排版错误。

- **多语言内容本地化协调**：本地化团队在翻译视频字幕或界面文案时，常需要对照多个原始语料源（如日文原版视频、英文对照字幕）。本系统可按语言或地区对链接进行分组，并为每个译员生成专属的链接速查表，提升协作效率。

- **历史资源归档与校验**：运营老牌内容站点的团队往往积累了大量数年前收录的外部链接。通过本项目的可用性嗅探功能，可定期对全量链接进行可达性扫描，快速定位失效或迁移的资源，及时更新或移除。

## 快速开始

以下步骤将在本地环境中完成 HyperLink Navigator 的克隆、依赖安装及基础运行。

```bash
# 1. 克隆仓库至本地
git clone https://github.com/your-org/hyperlink-navigator.git
cd hyperlink-navigator

# 2. 安装所需依赖（基于 Python 3.10+）
pip install -r requirements.txt

# 3. 运行示例导入流程（使用提供的 sample_links.txt）
python nav_engine.py --input sample_links.txt --output nav_output.md
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.10 及以上 | 核心脚本运行环境，用于链接解析与导出 |
| pip | 22.0 及以上 | Python 包管理工具，用于安装依赖库 |
| requests | 2.28.0 及以上 | 用于可选的可达性嗅探功能（发送 HEAD 请求） |
| pytest | 7.0.0 及以上 | 仅开发测试时需要，用于执行单元测试套件 |
| Git | 2.30.0 及以上 | 用于克隆仓库及版本管理（非运行必需） |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门 | [docs/quickstart.md](docs/quickstart.md) | 如何用最短时间完成第一批链接的导入与导出？ |
| 配置 | [docs/configuration.md](docs/configuration.md) | 如何自定义分类标签、输出模板及嗅探超时时间？ |
| 高级 | [docs/advanced-workflow.md](docs/advanced-workflow.md) | 如何结合 Git hooks 实现链接变更的自动校验？ |
| 参考 | [docs/api-reference.md](docs/api-reference.md) | 各个核心函数与命令行参数的详细说明 |

## 资源列表

以下资源属于本批次（第 11/130 批）收录的外部链接，内容主题涵盖免费影视观看、日韩视频资源及中文字幕相关站点，已按内容类别进行分组整理。

视频观看类（综合）

<code>https://mianfeiguankanzaixianguankan.org.cn</code>

<code>https://mianfeigaoqingshipinzaixianguankan.org.cn</code>

视频观看类（特定来源）

<code>https://jiujiushipinzaixianguankan.org.cn</code>

<code>https://oumeizaixianguankanshipin.org.cn</code>

<code>https://rihanshipinmianfeizaixianguankan.org.cn</code>

字幕与辅助资源类

<code>https://renqixiliezhongwenzimuw.org.cn</code>

<code>https://rihanmeinvzhongwenzimu.org.cn</code>

## 项目结构

```
hyperlink-navigator/
├── nav_engine.py          # 主入口脚本，整合导入、处理与导出流程
├── requirements.txt       # Python 依赖声明文件
├── pytest.ini             # 单元测试配置文件
├── sample_links.txt       # 示例输入文件（含 10 条测试链接）
├── core/
│   ├── parser.py          # 链接解析器：去重、协议归一化、域名提取
│   ├── classifier.py      # 分类器：基于关键词与正则规则打标签
│   ├── sniffer.py         # 可用性嗅探模块（基于 requests）
│   └── exporter.py        # 导出器：Markdown / JSON / HTML 生成
├── docs/
│   ├── quickstart.md      # 快速入门指南
│   ├── configuration.md   # 完整配置参数说明
│   ├── advanced-workflow.md # 高级工作流（CI/CD 集成）
│   └── api-reference.md   # 函数级 API 文档
├── tests/
│   ├── test_parser.py     # 解析器单元测试
│   ├── test_classifier.py # 分类器单元测试
│   └── test_sniffer.py    # 嗅探器单元测试（需网络）
└── output/                # 默认导出目录（自动生成，不入仓）
    └── (生成的导航文件)
```

## 贡献指南

1.  **提交问题或建议**：请在 GitHub Issues 中详细描述您遇到的问题或期望的新功能，并附上最小复现步骤或使用场景说明。对于链接资源类的变更，请注明具体批次编号。

2.  **实现功能或修复缺陷**：Fork 本仓库后，在您的分支上进行开发。请确保新增代码包含相应的单元测试（位于 `tests/` 目录），且所有现有测试用例均能通过。提交前请运行 `pytest` 进行验证。

3.  **完善文档**：若您发现文档中存在错漏或不清晰之处，欢迎直接修改对应的 `.md` 文件并提交 Pull Request。对于涉及资源列表章节的更新，请务必保持 URL 的原始格式不变。

## 常见问题

**问：导入的链接中包含大量不同格式的协议头（http、https、无协议），系统如何处理？**

答：解析器会自动识别并归一化处理。对于无协议的裸域名，默认补全为 `https://`；对于已带协议头的链接，保留原始协议。去重逻辑基于域名与路径的归一化形式，忽略协议差异，因此 `http://example.com` 与 `https://example.com` 会被视为同一资源。

**问：可用性嗅探功能是否会误判某些正常站点？**

答：嗅探器默认使用 `timeout=5` 秒的 HEAD 请求，仅检测 TCP 连接建立与服务器响应头返回情况。对于需要 JavaScript 渲染或强制 HTTPS 跳转的站点，HEAD 请求可能返回非 200 状态码，此时嗅探器会记录为“疑似异常”，但不会自动删除该链接。用户可根据输出报告人工复核。

**问：能否将本项目集成到现有的静态站点生成器（如 Hugo 或 Jekyll）中？**

答：可以。您只需将导出器格式设置为 Markdown 表格或 HTML 无序列表，并将生成的输出文件放置到站点内容目录下。对于批量更新场景，建议编写简单的 Shell 脚本在站点构建前自动运行 `nav_engine.py`。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
