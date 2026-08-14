# LinkHub Resource Aggregator

LinkHub is a community-driven technical resource aggregation platform designed to catalog, organize, and index high-value domain resources across multiple categories including media streaming, educational content, and real-time information services. The project serves as a curated directory for developers, researchers, and general users who need reliable access to specialized online resources without navigating through search engine noise or fragmented bookmark collections.

Target users include system administrators maintaining proxy lists, content researchers tracking media availability, and everyday users seeking structured access to entertainment and educational domains. LinkHub solves the fundamental problem of resource discoverability by maintaining a version-controlled, human-readable index that prioritizes availability verification and categorization over simple link dumping.

## 功能概览

- **Categorized Resource Indexing** – Organizes domains into functional groups such as streaming media, subtitle repositories, and educational platforms with metadata tags for quick filtering.

- **Automated Availability Health Checks** – Periodically tests each domain endpoint for HTTP response status, DNS resolution, and TLS certificate validity, flagging degraded resources.

- **Tag-Based Search and Filtering** – Supports full-text search across domain names, descriptions, and category labels with faceted filtering by status, region, and content type.

- **Community-Driven Update Workflow** – Accepts pull requests for new resource additions, updates, and removals with automated validation pipelines for URL formatting and accessibility.

- **Historical Change Audit Log** – Tracks every modification to the resource list with timestamp, author, and change reason, enabling rollback and transparency.

- **RESTful API Endpoint** – Exposes resource data in JSON format for integration with external monitoring dashboards, browser extensions, or mobile applications.

- **Static Site Generation Mode** – Builds a lightweight offline-capable HTML dashboard from the markdown source, suitable for deployment on CDN or local file systems.

## 应用场景

- **Media Availability Monitoring** – System administrators responsible for maintaining internal media gateways use LinkHub to quickly verify which streaming domains remain operational, reducing manual probing time from hours to seconds.

- **Educational Content Curation** – Educators and curriculum developers reference LinkHub to locate and share subtitle-enabled educational video sources for language learning courses, ensuring students have consistent access to supplementary materials.

- **Offline Resource Backup Planning** – Network operators in regions with intermittent connectivity use the exported JSON manifest to pre-fetch domain lists and configure local DNS overrides for critical services.

- **Research Data Collection** – Academic researchers studying domain registration patterns, content geo-restrictions, or streaming platform evolution leverage LinkHub's historical audit logs as primary data sources for longitudinal analysis.

- **Personal Bookmark Consolidation** – Individual users migrating between browsers or devices import LinkHub's structured list to replace scattered bookmark folders with a single, versioned resource master file.

## 快速开始

Clone the repository, install dependencies, and run the initial index build with the following commands:

```bash
git clone https://github.com/linkhub-community/linkhub.git
cd linkhub
pip install -r requirements.txt
python build_index.py --input resources.yml --output index.html
python serve.py --port 8080
```

The build process parses the YAML resource manifest, performs health checks against each domain, and generates both a static HTML dashboard and a JSON API endpoint. Use the `--watch` flag to enable live reload during development.

## 安装要求

| Dependency | Version Requirement | Purpose / Notes |
|------------|---------------------|-----------------|
| Python | 3.9 or higher | Core runtime for build scripts and API server |
| PyYAML | 6.0 or higher | Parsing resource manifest files in YAML format |
| requests | 2.28 or higher | Performing HTTP health checks with timeout handling |
| python-dotenv | 1.0 or higher | Loading environment variables for configuration |
| Flask | 2.2 or higher | Optional development web server for local preview |
| pytest | 7.0 or higher | Testing suite for validation pipelines (development only) |
| markdown | 3.4 or higher | Rendering README and documentation pages |
| jinja2 | 3.1 or higher | Template engine for static site generation |

## 文档导航

| Documentation Layer | Directory / File | Questions Answered |
|---------------------|------------------|---------------------|
| User Guide | docs/user-guide.md | How do I navigate the index? How do I interpret health status indicators? |
| Admin Manual | docs/admin-manual.md | How do I add or remove resources? How do I customize the build pipeline? |
| API Reference | docs/api-reference.md | What endpoints are available? What JSON schema is used for resource objects? |
| Contribution Workflow | CONTRIBUTING.md | How do I submit a new domain? What validation checks must pass before merge? |

## 资源列表

### Core Resource Index

<code>https://dejiabifenzhibo.org.cn</code>

<code>https://yijiabifenzhibo.org.cn</code>

<code>https://fajiabifenzhibo.org.cn</code>

### Media and Subtitle Resources

<code>https://guochanjingpinzaixianmianfeikan.org.cn</code>

<code>https://zhongwenzimuzaixianyingshiyuan.org.cn</code>

### Streaming and Viewing Platforms

<code>https://mianfeiguankanzaixianguankan.org.cn</code>

<code>https://jiujiushipinzaixianguankan.org.cn</code>

## 项目结构

```
linkhub/
├── src/
│   ├── core/                      # Core indexing and validation logic
│   │   ├── parser.py              # YAML manifest parser with schema validation
│   │   └── checker.py             # Health check orchestrator with concurrency
│   ├── web/                       # Web server and static generation modules
│   │   ├── app.py                 # Flask development server
│   │   └── generator.py           # Static HTML site builder
│   └── cli/                       # Command-line interface entry points
│       ├── build.py               # Build command implementation
│       └── serve.py               # Serve command with live reload
├── resources/
│   ├── manifest.yml               # Primary resource index (YAML format)
│   └── categories.yml             # Category definitions and tag mappings
├── docs/                          # End-user and admin documentation
│   ├── user-guide.md              # Navigation and usage instructions
│   └── admin-manual.md            # Maintenance and deployment guide
├── tests/                         # Unit and integration test suite
│   ├── test_parser.py             # Manifest parser test cases
│   └── test_checker.py            # Health check test mocks
├── output/                        # Generated artifacts (gitignored)
│   ├── index.html                 # Static dashboard build
│   └── api.json                   # JSON API endpoint output
├── templates/                     # Jinja2 HTML templates for site generation
│   ├── layout.html                # Base template with navigation
│   └── index.html                 # Resource listing template
├── requirements.txt               # Python dependency lock file
├── Makefile                       # Build automation targets
└── README.md                      # This document
```

## 贡献指南

1. **Fork and Clone** – Fork the main repository to your GitHub account, then clone your fork locally. Create a new branch with a descriptive name related to your intended change, such as `add-new-streaming-domain` or `update-health-check-timeout`.

2. **Modify the Manifest** – Edit the `resources/manifest.yml` file to add, update, or remove domain entries. Each entry must include the full URL, a short description, category tags, and a status reason if marking a domain as inactive. Run `make validate` to check schema compliance.

3. **Run Local Tests** – Execute the full test suite using `pytest tests/` to ensure your changes do not break existing functionality. Include new test cases in the appropriate test module if you are adding new features or modifying validation rules.

4. **Generate and Review** – Build the static site locally with `make build` and open `output/index.html` in your browser. Confirm that your changes appear correctly in the dashboard and that all links render with proper formatting.

5. **Submit Pull Request** – Push your branch to your fork and open a pull request against the main repository's `development` branch. In the PR description, reference the specific resources you modified and attach screenshots of the built dashboard if applicable. Maintainers will review within 48 hours.

## 常见问题

**Q: How frequently are the health checks performed and what status indicators are displayed?**

A: Health checks run automatically every 6 hours via a scheduled GitHub Action workflow. Each domain receives an HTTP HEAD request with a 5-second timeout. Status indicators include "Active" (2xx response), "Degraded" (4xx or 5xx response with retry), "Inactive" (timeout or DNS failure), and "Pending" (newly added, awaiting first check). Results are cached in the `output/api.json` file and displayed on the dashboard with color-coded badges.

**Q: What should I do if a domain I rely on is marked as inactive in the index?**

A: First, manually verify the domain using your browser or curl to confirm it is not a transient network issue on your side. If the domain is indeed accessible but the health check reported a failure, open an issue on GitHub with the domain URL and your verification results. If the domain is permanently unavailable, follow the contribution guide to submit a removal request or update the status reason. The maintainers periodically prune domains that remain inactive for 30 consecutive days.

**Q: Can I use LinkHub as a private internal resource aggregator without exposing my domain list publicly?**

A: Yes. The entire build pipeline works offline without any external telemetry or mandatory cloud dependencies. Clone the repository, modify the manifest to include your internal domains (which are never uploaded or transmitted), and run the build process on your local machine or internal CI server. The generated `output/` directory contains all assets required for serving the dashboard privately behind your own authentication layer. No data from your manifest is ever sent to external services unless you explicitly push to a remote repository.

## 许可证

MIT License. See the LICENSE file in the repository root for full terms and conditions.

> 外链数量: 7 | 生成时间: 2026-08-14 22:07:51
