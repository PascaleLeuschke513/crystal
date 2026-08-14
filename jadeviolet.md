# Domain Atlas

Domain Atlas is a comprehensive technical resource aggregator and domain intelligence platform designed for system administrators, security researchers, and infrastructure engineers. The project addresses the critical need for centralized, validated, and version-controlled access to domain metadata, WHOIS records, DNS propagation states, and SSL/TLS certificate intelligence across multiple top-level domains and registrar ecosystems.

Target users include DevOps engineers conducting infrastructure audits, security analysts performing threat intelligence correlation, network architects planning multi-region deployments, and compliance officers verifying domain ownership chains. Domain Atlas solves the fragmentation problem by providing a unified query interface, historical change tracking, and programmatic access to domain-associated metadata that would otherwise require querying dozens of disparate WHOIS servers, DNS resolvers, and certificate transparency logs.

## 功能概览

- **Bulk Domain Health Check** – Concurrently validate DNS resolution, WHOIS expiry, and SSL certificate validity across up to 10,000 domains with configurable timeout and retry policies.

- **Historical WHOIS Change Log** – Maintain immutable audit trails of registrar transfers, name server updates, and contact information modifications with ISO 8601 timestamped entries.

- **Certificate Transparency Monitor** – Poll public CT logs for newly issued certificates matching monitored domains, alert on unexpected issuers or weak signature algorithms.

- **DNS Propagation Tester** – Query 20+ global resolver nodes (including Google Public DNS, Cloudflare, Quad9, and regional providers) to measure geographic propagation consistency and TTL adherence.

- **Automated Screenshot Archiver** – Capture daily full-page screenshots of domain landing pages with configurable viewport dimensions and user-agent strings for visual regression detection.

- **RESTful API with Rate Limiting** – Expose all query capabilities via JSON-over-HTTP endpoints with token-based authentication, per-IP rate limiting, and response caching for high-frequency access patterns.

- **Exportable Reports** – Generate CSV, JSON, and PDF reports containing consolidated domain intelligence suitable for compliance submissions or internal documentation.

- **Watchlist with Webhook Notifications** – Define custom alert rules (expiry within 30 days, new subdomain detection, SSL revocation) with outbound Discord, Slack, and generic HTTP webhook delivery.

## 应用场景

- **Infrastructure Migration Pre-assessment** – Before relocating services to a new cloud provider, engineering teams use Domain Atlas to enumerate all dependent external domains, verify their DNS stability, and ensure certificate chains are trusted across the target region’s resolver infrastructure.

- **Phishing Domain Takedown Coordination** – Security incident response teams leverage the bulk query capabilities to investigate domain clusters sharing registrant email addresses or name server patterns, accelerating the identification of malicious infrastructure during active threat hunts.

- **Compliance Documentation for Audits** – Organizations subject to SOC2 or ISO 27001 controls utilize the historical change log and automated report generation to produce evidence of continuous domain governance and timely certificate renewal practices.

- **Startup Brand Protection Monitoring** – Legal and marketing teams configure watchlists for domain variants similar to their primary brand, receiving instant notifications when potentially infringing domains are registered or when SSL certificates are issued for those domains.

## 快速开始

Clone the repository, install Python dependencies, and run the initial domain ingestion pipeline using the following commands:

```bash
git clone https://github.com/domain-atlas/domain-atlas.git
cd domain-atlas
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
cp .env.example .env
# Edit .env to configure database connection and API keys
python manage.py migrate
python manage.py ingest --source examples/domain_list_sample.csv
python manage.py runserver --port 8080
```

## 安装要求

| Dependency | Version Requirement | Description |
|------------|---------------------|-------------|
| Python | 3.10 or higher | Core runtime; older versions lack async features used in resolver concurrency |
| PostgreSQL | 14.0 or higher | Primary database for storing domain records, historical changes, and user configurations |
| Redis | 7.0 or higher | In-memory cache for API response caching and distributed rate limiting across workers |
| dnspython | 2.4.0+ | Asynchronous DNS resolution library with EDNS0 support and DNSSEC validation |
| cryptography | 41.0.0+ | X.509 certificate parsing and signature verification for SSL monitoring |
| aiohttp | 3.9.0+ | HTTP client for CT log polling and webhook delivery with connection pooling |
| whois | 0.9.0+ | WHOIS protocol client with parser support for 50+ TLD-specific response formats |
| Pillow | 10.0.0+ | Image processing backend for automated screenshot capture and comparison |
| pytest | 7.4.0+ | Testing framework for unit and integration test suites (development dependency) |

## 文档导航

| Layer | Directory | Questions Addressed |
|-------|-----------|---------------------|
| User Guide | docs/user-guide/ | How do I set up watchlists? How do I interpret the health score? What export formats are supported? |
| API Reference | docs/api-reference/ | Which endpoints exist? What request/response schemas are defined? How does pagination work? |
| Deployment Guide | docs/deployment/ | How do I deploy with Docker Compose? What environment variables are required? How to set up a high-availability replica? |
| Troubleshooting | docs/troubleshooting/ | Why am I getting resolver timeouts? How do I resolve database migration conflicts? What logs should I check for certificate validation failures? |
| Contributor Guide | CONTRIBUTING.md | What coding style is enforced? How to run tests locally? What is the pull request review process? |

## 资源列表

The following external resources provide supplementary reference data, domain registration status, and validation sources that complement Domain Atlas functionality.

### Domain Registration and WHOIS References

<code>https://lanqiubifene.org.cn</code>

<code>https://lanqiubifenf.org.cn</code>

<code>https://lanqiubifeng.org.cn</code>

<code>https://lanqiubifenh.org.cn</code>

### Sports Statistics and Score References

<code>https://zuqiubifenziboa.org.cn</code>

<code>https://zuqiubifenzibob.org.cn</code>

<code>https://zuqiubifenziboc.org.cn</code>

## 项目结构

```
domain-atlas/
├── cmd/                                # Command-line entry points
│   ├── ingest/                         # Domain list ingestion routines
│   │   ├── csv_loader.py               # CSV parser with column auto-detection
│   │   └── validator.py                # Domain name syntax and IDNA validation
│   ├── monitor/                        # Background health check daemon
│   │   ├── scheduler.py                # APScheduler cron-based job coordinator
│   │   └── worker_pool.py              # Concurrent worker management with asyncio
│   └── api/                            # RESTful API server entry
│       ├── app.py                      # Flask/FastAPI application factory
│       └── middleware.py               # Rate limiting, CORS, and logging middleware
├── pkg/                                # Shared internal packages
│   ├── resolvers/                      # DNS, WHOIS, and CT log query implementations
│   │   ├── dns_engine.py               # Asynchronous DNS resolution with fallback
│   │   ├── whois_client.py             # WHOIS protocol wrapper with TLD-aware parsing
│   │   └── ct_monitor.py               # Certificate Transparency log poller
│   ├── storage/                        # Database models, migrations, and repositories
│   │   ├── models.py                   # SQLAlchemy ORM models for domains, records, alerts
│   │   ├── migrations/                 # Alembic versioned schema migrations
│   │   └── repositories.py             # Data access layer with query builders
│   ├── notifiers/                      # Webhook delivery and alerting engines
│   │   ├── webhook_dispatcher.py       # Generic HTTP/S webhook sender with retry
│   │   ├── discord_formatter.py        # Discord embed message builders
│   │   └── slack_formatter.py          # Slack block kit payload generators
│   └── utils/                          # Common utilities and helpers
│       ├── logger.py                   # Structured logging with JSON output
│       ├── config.py                   # Pydantic configuration loader
│       └── validators.py               # Domain, IP, and email format validators
├── tests/                              # Test suites
│   ├── unit/                           # Unit tests for individual functions and classes
│   ├── integration/                    # Integration tests requiring database and network
│   └── fixtures/                       # Sample domain lists and mock response data
├── docs/                               # User-facing and developer documentation
│   ├── user-guide/                     # Step-by-step usage tutorials
│   ├── api-reference/                  # OpenAPI/Swagger specification and endpoint details
│   └── deployment/                     # Docker, Kubernetes, and systemd deployment guides
├── scripts/                            # Maintenance and operational scripts
│   ├── backup_database.sh              # Postgres pg_dump with rotation
│   ├── migrate_schema.sh               # Alembic migration runner
│   └── seed_dev_data.py                # Development database population
├── config/                             # Environment-specific configuration files
│   ├── development.yaml                # Local development overrides
│   ├── staging.yaml                    # Staging environment variables
│   └── production.yaml                 # Production hardened settings
├── docker-compose.yml                  # Multi-container orchestration with Postgres, Redis, app
├── Dockerfile                          # Multi-stage container build definition
├── requirements.txt                    # Production Python dependencies pinned
├── requirements-dev.txt                # Development additional dependencies
├── pyproject.toml                      # PEP 621 project metadata and build configuration
├── .env.example                        # Environment variable template with placeholders
└── README.md                           # This file
```

## 贡献指南

1. Fork the repository and create a feature branch from `main` using the naming convention `feature/description` or `fix/issue-id`. Ensure your branch is rebased against the latest upstream `main` before starting work.

2. Write or adapt unit tests for any new functionality or bug fixes. All tests must pass with `pytest --cov=pkg --cov=cmd tests/` and coverage must not decrease below the current threshold of 85 percent.

3. Update documentation under the `docs/` directory to reflect your changes. For API modifications, regenerate the OpenAPI specification and ensure the user-guide examples remain accurate.

4. Submit a pull request with a clear description of the motivation, the approach taken, and any manual testing performed. Include screenshots for UI changes or sample API responses for backend modifications.

5. All pull requests must pass the continuous integration pipeline comprising linting, type checking, unit tests, and integration tests. A maintainer will review within two business days and may request additional changes or clarifications.

## 常见问题

**Q: How does Domain Atlas handle domains that are not registered or have expired WHOIS records?**

A: The WHOIS client implements exponential backoff and fallback to the IANA WHOIS server for generic TLDs. When a domain is unregistered or the WHOIS record is unavailable, the system records a `STATUS_UNREGISTERED` or `STATUS_WHOIS_TIMEOUT` state and retries with increasing intervals up to three attempts. All failures are logged with full context for subsequent manual verification.

**Q: Can Domain Atlas monitor private or internal DNS zones not exposed to the public internet?**

A: Yes, the DNS resolver supports custom resolver configuration via environment variables. Users can specify internal DNS servers (e.g., `CUSTOM_RESOLVERS=10.0.0.2,192.168.1.5`) and Domain Atlas will route queries to those resolvers first, falling back to public resolvers only when all custom endpoints fail. Split-horizon DNS configurations are fully supported by maintaining separate resolver profiles per monitored domain.

**Q: What are the performance characteristics for the bulk ingestion pipeline?**

A: In the default configuration, the ingestion pipeline processes approximately 500 domains per minute using 50 concurrent workers. Each worker handles WHOIS lookup, DNS A/AAAA/MX resolution, SSL certificate retrieval, and CT log query in parallel. Total runtime for a 10,000-domain list is under 20 minutes on a standard 4-core, 16GB instance. Pipeline throughput can be adjusted via the `WORKER_CONCURRENCY` and `INGEST_BATCH_SIZE` settings.

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-14 22:08:33
