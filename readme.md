# MindBet Prediction Market Demo

## Description

MindBet Prediction Market is an independent Linux-based environment for a self-hosted
prediction-market demo. It provides an isolated local runtime configuration for the
future frontend, API, MySQL, and Redis services while remaining separate from the
existing MindBet aggregation platform.

This directory is an independent Linux-side environment for the future self-hosted
MindBet Prediction Market demo. It is separate from the existing MindBet repository, Compose
project, network, containers, and data volumes.

The directory name intentionally contains a space:

```text
/home/ztmy/projects/mindbet prediction market
```

Always quote the path in shell commands.

## Scope

This initial directory contains configuration and resource placeholders only. It does
not contain application source code, domain behavior, trading logic, credentials, or
production deployment configuration.

The `frontend` and `api` directories are reserved for future application files. The
Compose `app` profile uses lightweight Nginx images as temporary runtime placeholders;
it is not an application implementation.

## Directory layout

```text
mindbet prediction market/
├── .env.example
├── .gitignore
├── docker-compose.yml
├── readme.md
├── api/                 # Future API application files
├── docs/                # Project documentation
├── frontend/            # Future frontend application files
├── mysql/
│   └── init/            # Optional first-start SQL files
└── redis/               # Reserved for Redis-related local configuration
```

## Runtime configuration

The Compose project name is `mindbet-prediction-market`. Its resources are isolated with a
dedicated bridge network and dedicated named volumes.

| Resource | Demo mapping | Container port | Notes |
| --- | ---: | ---: | --- |
| Frontend placeholder | `3400` | `80` | `app` profile only |
| API placeholder | `8188` | `80` | `app` profile only |
| MySQL 8.4 | `13307` | `3306` | Default infrastructure service |
| Redis 7.4 | `16379` | `6379` | Default infrastructure service |

These mappings are intentionally separate from the current MindBet local ports. The
demo does not join the current project's Docker network and does not reuse its
container names or volumes.

### Images and storage

- MySQL image: `mysql:8.4`
- Redis image: `redis:7.4-alpine`
- Temporary frontend image: `nginx:1.27-alpine`
- Temporary API image: `nginx:1.27-alpine`
- MySQL volume: `mindbet-prediction-market-mysql-data`
- Redis volume: `mindbet-prediction-market-redis-data`
- Network: `mindbet-prediction-market-network`

The MySQL and Redis volumes are persistent Docker-managed volumes. Removing them is
destructive and is not part of the normal stop workflow.

## Local prerequisites

- Linux or WSL2
- Docker Engine or Docker Desktop with WSL integration enabled
- Docker Compose v2
- Git, if this directory is managed as an independent repository

Docker is not required to inspect the directory structure, but it is required to run
the Compose resources.

## Setup

From Linux or WSL:

```bash
cd "/home/ztmy/projects/mindbet prediction market"
cp .env.example .env
```

Edit `.env` and replace the local placeholder passwords before starting MySQL. Do not
commit `.env`.

Validate the Compose configuration:

```bash
docker compose -p mindbet-prediction-market config
```

Start the infrastructure services:

```bash
docker compose -p mindbet-prediction-market up -d mysql redis
```

The temporary app placeholders can be started separately when needed:

```bash
docker compose -p mindbet-prediction-market --profile app up -d
```

Check service status and stop the project:

```bash
docker compose -p mindbet-prediction-market ps
docker compose -p mindbet-prediction-market down
```

`down` stops and removes the containers and network. It keeps the named data volumes.

## Configuration files

- `docker-compose.yml` declares service images, port mappings, health checks, network,
  and named volumes.
- `.env.example` documents the local port and database settings. Copy it to `.env` for
  local use.
- `mysql/init/` is reserved for optional first-start SQL files; it is empty by design.
- `frontend/`, `api/`, and `docs/` are reserved directories and contain no product
  implementation in this initial setup.

No server address, SSH key, database credential, or other secret is stored here.
