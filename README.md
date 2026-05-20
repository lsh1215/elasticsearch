[English](README.md) | [한국어](README-ko.md)

# Elasticsearch Study

![OpenSearch](https://img.shields.io/badge/OpenSearch_3-005EB8?style=flat-square&logo=opensearch&logoColor=white)
![OpenSearch Dashboards](https://img.shields.io/badge/OpenSearch_Dashboards-005EB8?style=flat-square&logo=opensearch&logoColor=white)
![Docker](https://img.shields.io/badge/Docker_Compose-2496ED?style=flat-square&logo=docker&logoColor=white)

A hands-on study repository for learning Elasticsearch concepts through OpenSearch-compatible experiments, documentation, and repeatable local environments.

## Table of Contents

- [Overview](#overview)
- [System Architecture](#system-architecture)
- [Tech Stack](#tech-stack)
- [Domain Model](#domain-model)
- [Key Design Decisions](#key-design-decisions)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [License](#license)

## Overview

This repository is for studying search-engine internals and Elasticsearch-style APIs by running small, documented experiments.

The current phase focuses on a local OpenSearch stack:

| Phase | Focus | Status |
|-------|-------|--------|
| 1 | Local OpenSearch and Dashboards runtime | Ready |
| 2 | Indexing, mappings, analyzers, and query DSL experiments | Planned |
| 3 | Performance notes, troubleshooting logs, and study documents | Planned |

OpenSearch is used because it preserves many Elasticsearch-compatible APIs while being easy to run locally with Docker.

## System Architecture

```text
+------------------+        HTTP :5601        +-------------------------+
| Local Browser    | -----------------------> | OpenSearch Dashboards   |
+------------------+                          | service: kibana         |
                                                +-----------+-------------+
                                                            |
                                                            | HTTP :9200
                                                            v
+------------------+        HTTP :9200        +-------------------------+
| curl / scripts   | -----------------------> | OpenSearch single node  |
+------------------+                          | service: opensearch     |
                                                +-----------+-------------+
                                                            |
                                                            v
                                                +-------------------------+
                                                | Docker named volume     |
                                                | opensearch-data         |
                                                +-------------------------+
```

## Tech Stack

![OpenSearch](https://img.shields.io/badge/OpenSearch_3-005EB8?style=flat-square&logo=opensearch&logoColor=white)
![OpenSearch Dashboards](https://img.shields.io/badge/OpenSearch_Dashboards-005EB8?style=flat-square&logo=opensearch&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![YAML](https://img.shields.io/badge/YAML-CB171E?style=flat-square&logo=yaml&logoColor=white)

| Category | Technology | Purpose |
|----------|------------|---------|
| Search engine | OpenSearch | Elasticsearch-compatible search experiments |
| Search UI | OpenSearch Dashboards | Explore indexes, queries, and cluster state |
| Runtime | Docker Compose | Reproducible local environment |
| Configuration | YAML / env vars | Local stack settings and version control |

## Domain Model

This repository does not define an application domain yet. Study experiments will introduce small indexes as needed, such as:

| Index | Purpose |
|-------|---------|
| `elasticsearch-study-notes` | Smoke-test and study-note documents |
| Future experiment indexes | Analyzer, mapping, ranking, and aggregation practice |

## Key Design Decisions

| Decision | Reason |
|----------|--------|
| Use OpenSearch images | Keeps the stack open and close to Elasticsearch-style APIs for study purposes |
| Run a single node | Reduces local setup cost while preserving core indexing and query workflows |
| Disable the security plugin locally | Avoids authentication friction for experiments on a private machine |
| Keep configuration in Docker Compose | Makes every experiment reproducible from a clean checkout |

## Getting Started

### Requirements

- Docker Desktop or a compatible Docker Engine
- Docker Compose v2
- At least 2 GB of available memory for the local stack

### Configure

Copy the example environment file if you want to pin or override values:

```bash
cp .env.example .env
```

Default values are already provided by `docker-compose.yml`, so this step is optional.

### Start

```bash
docker compose up -d
```

### Access URLs

| Service | URL |
|---------|-----|
| OpenSearch | http://localhost:9200 |
| OpenSearch Dashboards | http://localhost:5601 |

### Smoke Test

```bash
curl http://localhost:9200
curl 'http://localhost:9200/_cluster/health?pretty'
```

Index and search a sample document:

```bash
curl -X PUT http://localhost:9200/elasticsearch-study-notes/_doc/1 \
  -H 'Content-Type: application/json' \
  -d '{"topic":"docker-opensearch","note":"compose smoke test","status":"ok"}'

curl -X POST http://localhost:9200/elasticsearch-study-notes/_refresh
curl 'http://localhost:9200/elasticsearch-study-notes/_search?q=status:ok&pretty'
```

### Stop

```bash
docker compose down
```

## Project Structure

```text
.
├── .env.example
├── .gitignore
├── README.md
├── README-ko.md
└── docker-compose.yml
```

## License

No license has been selected yet. Add one before distributing or accepting external contributions.
