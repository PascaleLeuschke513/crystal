# Navigatrix Index

Navigatrix Index is a lightweight, community-driven technical resource aggregation and navigation system. It is designed for developers, researchers, and IT operations engineers who need to quickly locate authoritative, domain-specific reference materials without sifting through general-purpose search engine noise. The project does not host original content; instead, it provides a curated, machine-readable index of external technical resources, organized by domain, relevance, and update frequency. Navigatrix Index solves the problem of fragmented bookmarks and outdated internal wiki pages by offering a single, version-controlled, and scriptable entry point to essential external data sources used in daily development and troubleshooting workflows.

## 功能概览

- **Domain-Categorized Resource Indexing** – Each external link is tagged with one or more technical domains (e.g., network diagnostics, sports data APIs, certificate validation) for rapid filtering via CLI or web UI.

- **Automated Link Health Check** – A built-in scheduler periodically verifies HTTP status codes and TLS certificate validity of all indexed URLs, flagging broken or expired endpoints in a daily report.

- **Markdown-Based Configuration** – All resource entries are stored in plain Markdown files, allowing full Git-based history, pull request reviews, and offline editing without a database.

- **RESTful Query API** – Exposes a read-only JSON API over HTTP for programmatic access, supporting exact-match and prefix-based lookups on domain names or category labels.

- **Custom Metadata Attachment** – Each URL can carry custom key-value pairs (e.g., `region=cn`, `protocol=https`, `rate-limit=1000`) to enable fine-grained routing or monitoring rules in downstream automation.

- **Static Site Generation Mode** – Optionally compiles the index into a fully static HTML dashboard with search and sort capabilities, suitable for deployment on CDN or internal file servers.

- **Slack/Email Alert Integration** – Sends notifications to configured channels when a critical resource (marked `priority=high`) becomes unreachable for more than five consecutive checks.

## 应用场景

- **DevOps Pipeline Dependency Verification** – Before deploying a production release, the CI pipeline queries Navigatrix Index to verify that all external API endpoints and license servers are reachable, reducing deployment failures caused by external service changes.

- **Internal Technical Wiki Supplement** – Team documentation can reference Navigatrix Index as the single source of truth for external links, ensuring that every referenced URL is centrally monitored and updated when upstream changes occur.

- **Security Audit Trail** – Security teams use the index to maintain a list of third-party domains used by internal applications. The health check logs provide a historical record of connectivity and certificate issues for compliance reporting.

- **Regional Network Testing** – Network engineers filter resources by geographic metadata to test routing policies and latency from different PoP locations, using the API to retrieve a curated list of test endpoints.

- **Sports Data Aggregator Prototyping** – Data analysts prototyping ETL pipelines for real-time match results can use the indexed football score endpoints as stable, well-documented data sources during the initial development phase.

## 快速开始

Clone the repository, install dependencies, and run the local development server. All commands assume a POSIX-compatible shell environment.

```bash
# Clone the main repository from the upstream source
git clone https://github.com/navigatrix/index.git navigatrix-index
cd navigatrix-index

# Install required Python packages and local tooling
pip install -r requirements.txt
make setup

# Start the development server on localhost:8080
python -m navigatrix.server --port 8080 --config config/dev.yaml
```

After the server starts, open `http://localhost:8080` in your browser to view the web dashboard or use `curl http://localhost:8080/api/v1/resources` to test the JSON API.

## 安装要求

The following table lists all mandatory dependencies and system requirements for running Navigatrix Index in production or development mode. All versions are specified as minimum compatible versions; newer patch releases are generally accepted.

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Python | >= 3.9 | Core runtime; type hints and async features require 3.9+ |
| pip | >= 21.0 | Package installer for Python dependencies |
| Git | >= 2.25 | Required for version control operations and clone |
| SQLite | >= 3.35 | Embedded database for metadata cache and health history |
| curl | >= 7.68 | Used by health check worker for HTTP probing |
| make | >= 4.2 | Build automation for setup and test tasks |
| openssl | >= 1.1.1 | TLS certificate validation and key generation |
| pytest | >= 7.0 (dev) | Test framework; only needed for development environment |
| redis | >= 6.0 (optional) | Recommended for high-availability health check queues |

## 文档导航

The documentation is organized into four primary layers, each addressing a specific audience and set of questions. All documentation files are located in the `docs/` directory of the repository.

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | `docs/user/` | How do I add a new resource? How do I filter by category? How do I generate a static site? |
| 运维指南 | `docs/ops/` | How do I configure health check intervals? How do I set up alerting? Which ports need to be open? |
| API 参考 | `docs/api/` | What endpoints are available? What query parameters are supported? What does the response schema look like? |
| 贡献者指引 | `docs/contrib/` | What is the coding style? How do I write tests? What is the PR review process? |

## 资源列表

The following external resources are indexed and monitored by Navigatrix Index. They are grouped by functional category for easier navigation. Each URL is reproduced exactly as provided by the upstream data source, without any modification to scheme, subdomain, or path.

### 域名验证与证书状态资源

- <code>https://xijiajishibifena.org.cn</code>
- <code>https://dejiajishibifena.org.cn</code>
- <code>https://yijiajishibifena.org.cn</code>
- <code>https://fajiajishibifena.org.cn</code>

### 足球赛事数据资源

- <code>https://zuqiubisaijieguoa.org.cn</code>
- <code>https://yingchaobifena.org.cn</code>

### 综合比分与统计资源

- <code>https://xijiabifena.org.cn</code>

## 项目结构

The project follows a modular monolith architecture with clear separation between core indexing logic, health checking, API serving, and static generation. Below is the directory tree with annotations for each major component.

```
navigatrix-index/
├── config/                           # Environment-specific configuration files
│   ├── dev.yaml                      # Developer defaults with verbose logging
│   ├── prod.yaml                     # Production settings with reduced log level
│   └── schema.yaml                   # JSON schema for configuration validation
├── src/
│   └── navigatrix/                   # Main Python package
│       ├── __init__.py
│       ├── server.py                 # FastAPI application entry point
│       ├── indexer/                  # Resource indexing and parsing logic
│       │   ├── loader.py             # Reads Markdown files and extracts entries
│       │   └── registry.py           # In-memory registry with category indexes
│       ├── health/                   # Health check subsystem
│       │   ├── checker.py            # Asynchronous HTTP/TLS verifier
│       │   ├── scheduler.py          # Cron-like job scheduler
│       │   └── reporter.py           # Generates daily health summaries
│       ├── api/                      # RESTful API routes and handlers
│       │   ├── v1.py                 # Version 1 endpoint definitions
│       │   └── models.py             # Pydantic response schemas
│       └── static/                   # Static site generator
│           ├── builder.py            # Renders HTML from Jinja2 templates
│           └── assets/               # CSS and client-side JavaScript
├── tests/                            # Unit and integration tests
│   ├── unit/                         # Isolated tests for individual functions
│   └── integration/                  # End-to-end API and health check tests
├── docs/                             # Full documentation (see Documentation Navigation)
├── scripts/                          # Utility scripts for maintenance
│   ├── migrate_db.py                 # Schema migration tool for SQLite
│   └── seed_index.py                 # Initializes the index from a seed file
├── requirements.txt                  # Production Python dependencies
├── requirements-dev.txt              # Additional dependencies for development
├── Makefile                          # Common tasks: setup, test, run, clean
└── README.md                         # This file
```

## 贡献指南

We welcome contributions of all types, including new resource entries, bug fixes, documentation improvements, and feature proposals. Please follow the steps below to ensure a smooth contribution process.

1.  **Fork the Repository and Create a Feature Branch** – Fork the upstream repository to your personal GitHub account, then create a new branch with a descriptive name (e.g., `feature/add-sports-category`, `fix/health-check-timeout`). Use `git checkout -b branch-name` to switch to the new branch.

2.  **Implement Changes with Tests** – Make your changes in the codebase or documentation. For any functional change, add corresponding unit or integration tests under the `tests/` directory. Ensure all existing tests pass by running `make test`. Update the relevant documentation pages if your change affects user-facing behavior.

3.  **Update the Resource Index (if applicable)** – If your contribution adds, removes, or modifies external URLs, edit the appropriate Markdown files under `data/` (or the designated index directory). Follow the existing metadata format precisely. Run `make validate` to verify that all entries are syntactically correct.

4.  **Submit a Pull Request** – Push your branch to your fork and open a pull request against the upstream `main` branch. In the PR description, clearly reference the issue number (if any) and provide a step-by-step explanation of your changes. Include screenshots for UI changes or sample API responses for backend changes.

5.  **Address Review Feedback** – Maintainers will review your pull request within two business days. Address any comments or requested changes by committing additional fixes to your branch. Once all checks pass and at least one maintainer has approved, your PR will be merged.

## 常见问题

**Q: How often does the health check run, and can I customize the schedule?**

A: By default, the health checker runs every 60 minutes for all indexed resources. You can customize the interval globally in the `config/prod.yaml` file under the `health.interval_minutes` key. For per-resource overrides, add a `check_interval` key in the metadata section of the corresponding Markdown entry. The scheduler supports cron-like expressions for advanced scheduling needs.

**Q: Does Navigatrix Index store any personal data or user activity logs?**

A: No. Navigatrix Index does not collect, store, or transmit any personal data. It only caches HTTP response metadata (status, latency, TLS expiry) for the external URLs listed in the index. All cached data is stored locally in SQLite and is never sent to external services. The project is GDPR-compliant by design and requires no cookie consent banners.

**Q: Can I run Navigatrix Index behind a corporate proxy or in an air-gapped environment?**

A: Yes. For corporate proxy environments, set the standard `HTTP_PROXY` and `HTTPS_PROXY` environment variables before starting the server. For air-gapped environments, you can pre-populate the SQLite cache with health data from a trusted mirror and run the index in offline mode by setting `offline=true` in the configuration. In offline mode, the health checker is disabled, but the API and static site remain fully functional.

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
