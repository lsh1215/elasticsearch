[한국어](README.md) | [English](README-en.md)

# Elasticsearch Study

![OpenSearch](https://img.shields.io/badge/OpenSearch_3-005EB8?style=flat-square&logo=opensearch&logoColor=white)
![OpenSearch Dashboards](https://img.shields.io/badge/OpenSearch_Dashboards-005EB8?style=flat-square&logo=opensearch&logoColor=white)
![Docker](https://img.shields.io/badge/Docker_Compose-2496ED?style=flat-square&logo=docker&logoColor=white)

Elasticsearch 개념을 OpenSearch 호환 실험으로 직접 확인하고, 문서와 기록으로 남기기 위한 학습 저장소입니다.

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

이 저장소는 검색 엔진 내부 동작, Elasticsearch 스타일 API, 인덱싱과 쿼리 흐름을 작은 실험 단위로 학습하기 위해 사용합니다.

현재 단계는 로컬 OpenSearch 실행 환경을 만드는 데 집중합니다.

| Phase | Focus | Status |
|-------|-------|--------|
| 1 | 로컬 OpenSearch와 Dashboards 실행 환경 | Ready |
| 2 | 인덱싱, 매핑, 애널라이저, Query DSL 실험 | Planned |
| 3 | 성능 기록, 문제 해결 로그, 학습 문서 정리 | Planned |

OpenSearch는 Elasticsearch와 호환되는 API를 많이 제공하면서 Docker로 간단히 실행할 수 있어 학습용 런타임으로 사용합니다.

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
| 검색 엔진 | OpenSearch | Elasticsearch 호환 검색 실험 |
| 검색 UI | OpenSearch Dashboards | 인덱스, 쿼리, 클러스터 상태 확인 |
| 실행 환경 | Docker Compose | 재현 가능한 로컬 환경 구성 |
| 설정 | YAML / env vars | 로컬 스택 설정과 버전 관리 |

## Domain Model

아직 별도의 애플리케이션 도메인은 정의하지 않았습니다. 실험 주제에 따라 작은 인덱스를 추가해 사용할 예정입니다.

| Index | Purpose |
|-------|---------|
| `elasticsearch-study-notes` | 스모크 테스트와 학습 메모 문서 |
| 향후 실험용 인덱스 | 애널라이저, 매핑, 랭킹, 집계 연습 |

## Key Design Decisions

| Decision | Reason |
|----------|--------|
| OpenSearch 이미지 사용 | Elasticsearch 스타일 API를 학습하기 쉽고 공개적으로 실행 가능한 스택 구성 |
| 싱글 노드 구성 | 로컬 실행 부담을 낮추면서 핵심 색인/검색 흐름을 유지 |
| 로컬 보안 플러그인 비활성화 | 개인 개발 환경에서 인증보다 실험 속도를 우선 |
| Docker Compose에 설정 집중 | 깨끗한 체크아웃에서도 같은 환경을 재현 |

## Getting Started

### Requirements

- Docker Desktop 또는 호환 가능한 Docker Engine
- Docker Compose v2
- 로컬 스택 실행을 위한 여유 메모리 2 GB 이상

### Configure

값을 고정하거나 덮어쓰고 싶다면 예시 env 파일을 복사합니다.

```bash
cp .env.example .env
```

기본값은 `docker-compose.yml`에 이미 들어 있어 이 단계는 선택 사항입니다.

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

샘플 문서를 색인하고 검색합니다.

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
├── README-en.md
└── docker-compose.yml
```

## License

아직 라이선스를 선택하지 않았습니다. 외부 공개나 기여를 받기 전 라이선스를 추가하세요.
