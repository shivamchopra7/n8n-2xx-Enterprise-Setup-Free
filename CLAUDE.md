# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Docker Compose-based setup for running n8n (workflow automation) in an enterprise configuration with queue-mode execution. It is **not** an application codebase — it is infrastructure configuration (Docker Compose, shell scripts, Nginx config, environment templates).

The stack runs: **n8n (beta/next)** + **PostgreSQL 15** + **Redis 7** + **Nginx Proxy Manager** + **scalable n8n workers**.

## Key Commands

```bash
# Initial setup (generates .env with secure passwords/keys)
./scripts/setup.sh

# Start all services
docker compose up -d

# Stop all services
docker compose down

# View logs (all or specific service)
docker compose logs -f
docker compose logs -f n8n

# Scale workers
docker compose up -d --scale n8n-worker=5

# Health check
./scripts/health-check.sh

# Backup database
docker compose exec postgres pg_dump -U n8n n8n > backup.sql
```

## Architecture

### Service Dependency Chain

```
nginx-proxy-manager → n8n (main) → postgres (healthy)
                                  → redis (healthy)
                      n8n-worker  → n8n (started)
                                  → postgres (healthy)
                                  → redis (healthy)
```

### Docker Compose Structure

[docker-compose.yaml](docker-compose.yaml) uses a YAML anchor (`x-n8n-service: &n8n_service`) to share configuration between the main n8n service and workers. Both use the same image (`n8nio/n8n:next`), volumes, and base environment — the worker overrides `command: worker` and adds `EXECUTIONS_MODE=queue`.

All services share a single bridge network (`n8n_network`). Services reference each other by container name (e.g., `DB_POSTGRESDB_HOST=postgres`, `QUEUE_BULL_REDIS_HOST=redis`).

### Ports

| Service | Port |
|---|---|
| n8n editor/webhooks | 5678 |
| PostgreSQL | 5432 |
| Redis | 6379 |
| Nginx Proxy Manager admin | 81 |
| HTTP/HTTPS (via NPM) | 80/443 |

### Key Files

- [docker-compose.yaml](docker-compose.yaml) — service orchestration, health checks, logging config
- [.env.example](.env.example) — all configurable environment variables with documentation
- [scripts/setup.sh](scripts/setup.sh) — interactive setup: generates encryption keys, passwords, optional SMTP config
- [scripts/health-check.sh](scripts/health-check.sh) — validates all services, ports, and resource usage
- [nginx/load-balancer/n8n-load-balancer.conf](nginx/load-balancer/n8n-load-balancer.conf) — optional advanced Nginx config with rate limiting and security headers

### Configuration

All runtime configuration flows through `.env` (created from `.env.example`). The docker-compose file reads this via `env_file: .env` and uses `${VAR:-default}` syntax for fallbacks. Key config areas: database credentials, Redis password, n8n host/protocol/auth, worker concurrency (`N8N_WORKER_CONCURRENCY`), execution pruning, and optional SMTP.
