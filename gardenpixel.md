# MetaHub Resource Aggregator

MetaHub Resource Aggregator is a comprehensive technical documentation and resource navigation system designed for developers, researchers, and system administrators who need to centrally manage, categorize, and access distributed technical references, API documentation, and operational guides. The project addresses the fragmentation of technical resources across multiple domains by providing a unified, version-controlled, and searchable metadata repository that integrates external links, internal knowledge bases, and real-time status dashboards.

Target users include DevOps engineers managing multi-environment deployments, backend developers integrating third-party services, technical writers maintaining documentation ecosystems, and site reliability engineers monitoring distributed systems. The aggregator does not host content but serves as a structured index with validation hooks, link freshness checks, and contextual annotations to ensure every referenced resource remains actionable and relevant.

## 功能概览

- **Multi-Source Link Aggregation** - Centralized collection of technical references, official documentation, and community resources with automatic categorization based on domain patterns and content type detection.

- **Link Health Monitoring** - Periodic HTTP status checks and SSL certificate expiration warnings for all registered external URLs, with failure notifications routed through configurable webhook integrations.

- **Tag-Based Classification System** - Hierarchical tagging engine supporting multiple taxonomies including technology stack, geographic region, deployment environment, and operational status.

- **Full-Text Search with Filters** - Search across resource titles, descriptions, tags, and domain names with faceted filtering by category, status code, and last verification timestamp.

- **Versioned Metadata Storage** - Every resource entry maintains a change log with timestamp, modifier identity, and diff view, enabling rollback and audit trail capabilities.

- **Batch Import and Export** - Support for bulk URL ingestion via CSV, JSON, and plain text line-delimited formats, with validation rules to reject malformed or unreachable endpoints.

- **RESTful API Gateway** - Programmatic access to all aggregation functions with token-based authentication, rate limiting, and response caching for high-frequency query scenarios.

- **Dashboard Visualization** - Built-in administrative panel showing resource distribution charts, health status summary, trend graphs of link additions, and top domains by usage frequency.

## 应用场景

- **Microservices Documentation Hub** - A development team maintains a centralized portal linking to service-specific Swagger UI instances, health check endpoints, and deployment runbooks across staging and production environments. The aggregator provides a single entry point for all team members, reducing context switching and documentation discovery time.

- **Multi-Region Compliance Reference** - A fintech company tracks regulatory API endpoints and financial data sources across different jurisdictions. Each resource is tagged with region codes, compliance tier, and refresh schedule. The monitoring feature alerts the compliance officer when any critical link becomes unreachable.

- **Open Source Dependency Tracking** - A project maintainer aggregates links to upstream project repositories, issue trackers, discussion forums, and mirror download sites. The health check ensures that build scripts and CI pipelines always reference live resources, preventing broken artifact downloads.

- **Incident Response Playbook** - An SRE team curates a list of internal dashboards, logging interfaces, and emergency escalation channels. During an outage, the aggregator offers quick navigation to all relevant tools without memorizing obscure internal hostnames or IP addresses.

- **Academic Research Bibliography** - A research group organizes links to preprints, dataset repositories, supplementary code repositories, and lab experiment logs. The tagging system allows filtering by publication year, research topic, and author affiliation.

## 快速开始

```bash
# Step 1: Clone the repository
git clone https://github.com/metahub/aggregator.git
cd aggregator

# Step 2: Install dependencies using pipenv or virtualenv
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt

# Step 3: Initialize the local metadata database and run the development server
python manage.py migrate
python manage.py load_initial_resources
python manage.py runserver --host 0.0.0.0 --port 8000
```

After the server starts, navigate to `http://localhost:8000/dashboard` to access the administrative interface. The default credentials are `admin` / `changeme` (change immediately in production). Use `python manage.py import_urls --file sample_urls.txt` to import your initial list of resources.

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.9 - 3.11 | Core runtime; 3.12 is not yet fully supported due to dependency compatibility issues |
| PostgreSQL | 13.0+ | Primary metadata store; supports full-text search and JSONB fields for flexible schema |
| Redis | 6.0+ | Optional caching layer for API responses and health check job queue backend |
| Node.js | 16.x or 18.x | Required only for frontend asset compilation in development mode |
| docker-compose | 2.0+ | Recommended for local development environment orchestration |
| curl | 7.68+ | Used by health check worker to perform HTTP probe requests |
| git | 2.25+ | Version control and automated resource update logging |

Additional Python packages are listed in `requirements.txt` and include FastAPI, SQLAlchemy, Pydantic, Alembic, Celery, and httpx. The system supports SQLite for testing but PostgreSQL is strongly recommended for production deployments with more than 10,000 resource entries.

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户指南 | `/docs/user-guide/getting-started.md` | How do I set up my first resource list, add URLs, and view the dashboard? |
| 操作手册 | `/docs/operations/health-check-config.md` | How do I configure health check intervals, timeout thresholds, and notification channels? |
| 开发参考 | `/docs/development/api-endpoints.md` | Which API endpoints are available for programmatic resource management and how do I authenticate? |
| 部署指南 | `/docs/deployment/kubernetes-helm.md` | How do I deploy the aggregator on Kubernetes with persistent storage and TLS termination? |
| 故障排除 | `/docs/troubleshooting/common-errors.md` | What does error code E503 mean and how do I resolve database migration conflicts? |
| 贡献规范 | `/docs/contributing/coding-standards.md` | What are the Python style guidelines, commit message conventions, and PR review criteria? |

All documentation is written in Markdown and versioned alongside the source code. The `/docs` root contains an index file that serves as the entry point for both new and experienced users.

## 资源列表

### Primary Resource Domains

<code>https://yingchaobifenc.org.cn</code>

<code>https://xijiabifenc.org.cn</code>

<code>https://dejiabifenc.org.cn</code>

<code>https://yijiabifenc.org.cn</code>

<code>https://fajiabifenc.org.cn</code>

### Live Streaming and Real-Time Data Sources

<code>https://yingchaobifenzhibo.org.cn</code>

<code>https://xijiabifenzhibo.org.cn</code>

Each of these URLs is treated as a first-class resource entity within the aggregator. The system periodically probes each endpoint, records HTTP status codes, response times, and TLS handshake details. All resources share a common metadata schema including title, description, category tags, and operational notes. The streaming domains are automatically assigned a "realtime" tag and a higher health check frequency (every 60 seconds) compared to standard domains (every 300 seconds).

## 项目结构

```
aggregator/
├── src/
│   ├── core/                     # Application core: settings, logging, dependency injection
│   │   ├── config.py             # Environment-specific configuration with Pydantic
│   │   ├── database.py           # SQLAlchemy engine, session factory, and base model
│   │   └── celery_app.py         # Celery instance for asynchronous task processing
│   ├── resources/                # Resource management module
│   │   ├── models.py             # ORM models: Resource, Tag, HealthCheckLog, ChangeHistory
│   │   ├── schemas.py            # Pydantic schemas for request/response validation
│   │   ├── service.py            # Business logic: CRUD, search, import/export, tagging
│   │   └── health.py             # Health check worker: HTTP probe, SSL verification, status update
│   ├── api/                      # RESTful API layer
│   │   ├── routes.py             # FastAPI route definitions with dependency injection
│   │   ├── middleware.py         # CORS, logging, authentication, rate limiting
│   │   └── validators.py         # Custom URL validation and normalization utilities
│   ├── dashboard/                # Administrative frontend (React + Vite)
│   │   ├── components/           # Reusable UI components: tables, charts, filters
│   │   ├── pages/                # Page-level components: Dashboard, ResourceList, DetailView
│   │   └── hooks/                # Custom React hooks for API calls and state management
│   └── cli/                      # Command-line tools
│       ├── import_cmd.py         # Batch import from CSV, JSON, or plain text
│       ├── export_cmd.py         # Export resources to various formats with filtering
│       └── health_cmd.py         # Manual health check trigger and report generation
├── tests/                        # Unit and integration tests
│   ├── unit/                     # Isolated tests for models, services, validators
│   └── integration/              # API endpoint tests, database transaction tests
├── docs/                         # Full documentation suite (see Document Navigation)
├── scripts/                      # Maintenance scripts: backup, migration, seed data
├── deployments/                  # Deployment manifests: Dockerfiles, kubernetes yamls
├── requirements.txt              # Production Python dependencies
├── requirements-dev.txt          # Development dependencies: pytest, black, mypy, pre-commit
├── docker-compose.yml            # Local stack: PostgreSQL, Redis, app, worker
├── Makefile                      # Common tasks: test, lint, format, migrate, run
└── README.md                     # This file
```

## 贡献指南

1. **Fork and Clone** - Fork the repository on GitHub, clone your fork locally, and set up the upstream remote to track changes from the main repository. Use `git remote add upstream https://github.com/metahub/aggregator.git` to enable syncing.

2. **Create a Feature Branch** - Base your work on the `develop` branch. Use a descriptive branch name following the pattern `feature/short-description` or `fix/issue-number-description`. For example, `feature/add-url-validation-regex` or `fix/health-check-timeout`.

3. **Write Tests and Documentation** - Every new feature or bug fix must include corresponding unit tests in the `tests/` directory. Update the relevant documentation files in `/docs` to reflect changes in behavior or configuration. Run `make test` to ensure all tests pass locally.

4. **Run Code Quality Checks** - Execute `make lint` to run flake8, mypy, and black in check mode. Use `make format` to automatically apply code formatting. Ensure there are no type errors or style violations before committing.

5. **Submit a Pull Request** - Push your branch to your fork and open a pull request against the `develop` branch of the main repository. Include a clear description of the changes, reference any related issues, and mark the PR as ready for review after all CI checks pass. The maintainers will review your submission within 5 business days.

## 常见问题

**Q: How does the aggregator handle URL changes or redirections?**

The health check worker follows up to three redirects and records the final destination URL in the `redirect_target` field of the health log. If a permanent redirect (HTTP 301 or 308) is detected, the system automatically updates the resource entry to point to the new location after three consecutive observations of the same redirect target. Temporary redirects (302, 307) are logged but do not trigger automatic updates. The dashboard displays a redirect warning icon for resources that have changed their effective endpoint.

**Q: Can I restrict access to certain resources based on user roles?**

Yes, the aggregator integrates with an external OAuth2 provider (Keycloak or Okta) and supports attribute-based access control. Resources can be tagged with `public`, `internal`, or `restricted` visibility levels. Users assigned to the `viewer`, `editor`, or `admin` roles have different permissions for viewing, editing, and deleting resources. The API endpoints enforce these permissions using dependency injection and JWT claims. Configuration for role mapping is available in `src/core/config.py`.

**Q: What happens when a resource health check consistently fails?**

After five consecutive failures, the resource status is set to `DEGRADED`. After 20 consecutive failures, the status transitions to `UNREACHABLE`. The system sends an alert via the configured notification channel (email, Slack, or PagerDuty) at the 5th, 10th, and 20th failure. The aggregator also attempts to retrieve cached responses from the Internet Archive or local snapshot if available. Administrators can manually override the status and set a maintenance window to suppress alerts during scheduled downtime.

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
