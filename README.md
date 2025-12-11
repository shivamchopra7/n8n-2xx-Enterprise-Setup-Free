# n8n 2.0 Enterprise AI Setup (Latest)

<div align="center">

![n8n Enterprise AI Setup](https://img.shields.io/badge/n8n-Enterprise%20AI%20Setup-blue?style=for-the-badge&logo=n8n)
![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?style=for-the-badge&logo=docker)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-336791?style=for-the-badge&logo=postgresql)
![Redis](https://img.shields.io/badge/Redis-7-DC382D?style=for-the-badge&logo=redis)
![Nginx Proxy Manager](https://img.shields.io/badge/NPM-Latest-brightgreen?style=for-the-badge&logo=nginx)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

**Engineered & Repacked by [Shivam Chopra](https://github.com/shivamchopra7)**

</div>

---

## Overview

This project provides a comprehensive, self-contained **Docker Compose** setup for running **n8n (Enterprise AI/Beta)** locally or on a server. It is pre-configured with industry-standard tools to ensure performance, scalability, and security without any complex manual setup.

### Key Features

- **Self-Contained Environment**: Runs entirely on your machine using Docker. No external cloud dependencies.
- **High Performance**: Enabled **Queue Mode** using Redis to handle heavy workloads efficiently.
- **Scalable**: Easily add more background workers to handle thousands of workflows.
- **Secure & Easy SSL**: Includes **Nginx Proxy Manager (NPM)** to manage domains and SSL certificates with a simple web interface.
- **Latest n8n Version**: Configured to use the latest Beta features (`n8n:next`).
- **Production Ready Components**: Uses PostgreSQL for the database and Redis for caching/queuing.

---

## Architecture

This setup orchestrates several services to work together:

1.  **Nginx Proxy Manager (NPM)**: The entry point for all traffic. It handles SSL and routes requests to n8n.
2.  **n8n Main**: The main application where you create and edit workflows.
3.  **n8n Workers**: Background services that execute your workflows. You can run multiple workers to speed up processing.
4.  **PostgreSQL**: Sturdy database to store your workflows, credentials, and execution history.
5.  **Redis**: Manages the job queue, passing tasks from the main instance to workers.

---

## Installation

### Prerequisites

Ensure you have the following installed:
- **Docker Engine** (v20.10 or newer)
- **Docker Compose** (v2.0 or newer)
- **Hardware**: At least 4GB RAM (8GB recommended) and 20GB disk space.

### Quick Start

**Option 1: Automatic Setup (Recommended for Mac/Linux)**
```bash
# Clone the repository
git clone https://github.com/shivamchopra7/n8n-2xx-Enterprise-Setup-Free
cd n8n-2xx-Enterprise-Setup-Free

# Run the setup script
./scripts/setup.sh

# Start the services
docker compose up -d

# Verify everything is running
./scripts/health-check.sh
```

**Option 2: Manual Setup**
1.  **Prepare Configuration**:
    ```bash
    cp .env.example .env
    ```
2.  **Edit .env File**:
    - Open `.env` in a text editor.
    - Set a secure `N8N_ENCRYPTION_KEY`. You can generate one with: `openssl rand -base64 32`.
    - Set secure passwords for `POSTGRES_PASSWORD` and `REDIS_PASSWORD`.
3.  **Start Services**:
    ```bash
    docker compose up -d
    ```

---

## Configuration

### Nginx Proxy Manager (NPM)

NPM provides a user-friendly interface to manage your domains and SSL.

1.  **Access Admin UI**: Open `http://localhost:81` in your browser.
2.  **Default Login**:
    - Email: `admin@example.com`
    - Password: `changeme`
    *(You will be asked to change these immediately.)*
3.  **Add a Proxy Host**:
    - **Domain Names**: Enter your desired domain (e.g., `n8n.local` or `n8n.your-domain.com`).
    - **Scheme**: `http`
    - **Forward Hostname**: `n8n`
    - **Forward Port**: `5678`
    - **Block Common Exploits**: Enable this.
    - **Websockets Support**: Enable this (Required for n8n).
    - **SSL**: Request a certificate (e.g., Let's Encrypt) and enable "Force SSL".

### Email Setup (SMTP)

To send emails from n8n, configure the SMTP settings in your `.env` file.

**Gmail Example**:
```bash
N8N_SMTP_HOST=smtp.gmail.com
N8N_SMTP_PORT=587
N8N_SMTP_USER=your-email@gmail.com
N8N_SMTP_PASS=your-app-password  # Use an App Password, not your login password
N8N_SMTP_SENDER=your-email@gmail.com
N8N_SMTP_SSL=false
N8N_SMTP_STARTTLS=true
```
*(Support for Outlook, Yahoo, and custom SMTP is also available. Check `.env.example` for details.)*

---

## Usage Guide

### Common Commands

- **Start Services**: `docker compose up -d`
- **Check Status**: `docker compose ps`
- **View Logs**: `docker compose logs -f`
- **Stop Services**: `docker compose down`
- **Run Health Check**: `./scripts/health-check.sh`

### Scaling Workers

If your automations are slow or you have high volume, you can add more workers:

```bash
# Start 5 worker instances
docker compose up -d --scale n8n-worker=5

# Reduce back to 2 workers
docker compose up -d --scale n8n-worker=2
```

### Accessing Services

- **n8n Editor**: `http://localhost:5678` (or your configured domain)
- **NPM Admin**: `http://localhost:81`
- **Database**: `localhost:5432`
- **Redis**: `localhost:6379`

---

## Backup & Restore

It is important to back up your data regularly.

**Backup**:
```bash
# Create a backup folder
mkdir -p backups/$(date +%Y%m%d)

# Backup Database
docker compose exec postgres pg_dump -U n8n n8n > backups/$(date +%Y%m%d)/database.sql

# Backup n8n User Data (Workflows, Credentials)
docker cp $(docker compose ps -q n8n):/home/node/.n8n backups/$(date +%Y%m%d)/n8n-data
```

**Restore**:
To restore, you would load the SQL file back into Postgres and copy the n8n data folder back into the container.
(See `docker compose exec -T postgres psql ...` commands for restoration details).

---

## Troubleshooting

- **n8n Won't Start**: Check the logs (`docker compose logs n8n`). If the Beta version is unstable, switch the image in `docker-compose.yaml` to `n8nio/n8n:latest`.
- **Database Connection Error**: Ensure the container is healthy (`docker compose exec postgres pg_isready -U n8n`).
- **Port Conflicts**: If ports 80, 81, or 5432 are in use, stop the conflicting service or change the port mapping in `docker-compose.yaml`.

---

<div align="center">

**Ready to automate?**

```bash
docker compose up -d
```

**Access n8n at: http://localhost:5678**

**Built with ❤️ by [Shivam Chopra](https://github.com/shivamchopra7) for Superheros**

</div>