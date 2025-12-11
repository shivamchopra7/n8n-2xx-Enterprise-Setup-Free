# ⚡ n8n Enterprise AI Setup (Beta) - The Ultimate Automation Fortress

<div align="center">

![n8n Enterprise AI Setup](https://img.shields.io/badge/n8n-Enterprise%20AI%20Setup-blue?style=for-the-badge&logo=n8n)
![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?style=for-the-badge&logo=docker)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-336791?style=for-the-badge&logo=postgresql)
![Redis](https://img.shields.io/badge/Redis-7-DC382D?style=for-the-badge&logo=redis)
![Nginx Proxy Manager](https://img.shields.io/badge/NPM-Latest-brightgreen?style=for-the-badge&logo=nginx)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

**🦸‍♂️ Engineered by [Shivam Chopra](https://github.com/Shivam-2WaysTech) - The Automation Architect**

*"With great automation comes great responsibility"* - Uncle Ben (probably)

</div>

---

## 🌟 The Origin Story

Welcome to the **most powerful n8n setup in the multiverse**! This isn't just another Docker Compose file - it's a **re-engineered enterprise-grade automation fortress** running the **latest beta features** of n8n.

### 🚀 What Makes This Setup Legendary?

Born from the need to democratize enterprise automation, this setup breaks free from the cloud and gives you:

- **🏰 Fortress of Solitude**: Completely self-contained local environment
- **⚡ Lightning Speed**: **Queue Mode** enabled by default for blazing performance
- **🔄 Infinite Scaling**: Horizontal worker scaling that grows with your ambitions
- **🛡️ Impenetrable Security**: Nginx Proxy Manager (NPM) for easy SSL and access control
- **🎯 AI-Ready Arsenal**: Running `n8n:next` (Beta) for the cutting-edge features
- **📊 Command Center**: Built-in monitoring and health checks

### 💪 Superpowers Unlocked

✅ **Zero Cloud Dependencies** - Your data stays in your fortress
✅ **Enterprise-Grade Scaling** - Handle thousands of concurrent workflows
✅ **AI Agent Development** - Perfect platform for intelligent automation
✅ **Easy SSL & Proxy** - **Nginx Proxy Manager** with UI on port 81
✅ **Queue Management** - Redis-backed job processing
✅ **Health Monitoring** - Real-time system diagnostics
✅ **SMTP Email Support** - Native integration for Gmail, Outlook, Yahoo, and Custom SMTP

---

## 📋 Table of Contents

- [🌟 The Origin Story](#-the-origin-story)
- [🏗️ The Master Plan (Architecture)](#️-the-master-plan-architecture)
- [🛠️ Fortress Setup (Installation)](#️-fortress-setup-installation)
- [📧 Email Powers Activation (SMTP)](#-email-powers-activation-smtp)
- [⚡ Superpower Demonstrations (Usage)](#-superpower-demonstrations-usage)
- [🏛️ Command Center Blueprint (Structure)](#️-command-center-blueprint-structure)
- [🎯 The Arsenal (Advanced Features)](#-the-arsenal-advanced-features)
- [🆘 Emergency Protocols (Troubleshooting)](#-emergency-protocols-troubleshooting)

---

## 🏗️ The Master Plan (Architecture)

Behold the **Automation Fortress** - a carefully orchestrated symphony of services working in perfect harmony:

```mermaid
graph TB
    subgraph "🌐 External Access"
        USER[👤 Developer/User]
        BROWSER[🌍 Web Browser]
    end

    subgraph "🛡️ Secure Gateway"
        NPM[🛡️ Nginx Proxy Manager<br/>Ports 80/443 (Web)<br/>Port 81 (Admin UI)]
    end

    subgraph "🎯 Application Layer"
        N8N[🚀 n8n Main Instance<br/>Port 5678<br/>Web UI & API]
        WORKER1[⚙️ n8n Worker 1<br/>Background Jobs]
        WORKER2[⚙️ n8n Worker N<br/>Scalable Workers]
    end

    subgraph "💾 Data Layer"
        REDIS[🔴 Redis Queue<br/>Port 6379<br/>Job Management]
        POSTGRES[🐘 PostgreSQL<br/>Port 5432<br/>Data Storage]
    end

    USER --> BROWSER
    BROWSER --> NPM
    NPM --> N8N
    N8N --> WORKER1
    N8N --> WORKER2
    WORKER1 --> REDIS
    WORKER2 --> REDIS
    N8N --> POSTGRES
    WORKER1 --> POSTGRES
    WORKER2 --> POSTGRES
```

### 🎭 The Cast of Characters

| Service | Role | Port | Superpower |
|---------|------|------|------------|
| **🚀 n8n Main** | The Commander | 5678 | Web interface, API, workflow orchestration (Beta Version) |
| **🛡️ NPM** | The Gatekeeper | 80/443/81 | Easy Proxy Management & SSL |
| **⚙️ n8n Workers** | The Workforce | - | Background job processing, infinite scaling |
| **🐘 PostgreSQL** | The Memory Bank | 5432 | Persistent data storage, ACID compliance |
| **🔴 Redis** | The Speed Demon | 6379 | Lightning-fast queue management |

---

## 🛠️ Fortress Setup (Installation)

Ready to build your automation empire? Let's get this fortress operational! 🚀

### 🔧 Prerequisites (Assemble Your Tools)

```bash
# Required Arsenal
✅ Docker Engine 20.10+
✅ Docker Compose 2.0+
✅ 4GB+ RAM (8GB+ recommended)
✅ 20GB+ disk space
```

### 🚀 Quick Deploy

**Option 1: The Automated Hero Route** ⚡
```bash
# Clone the fortress
git clone https://github.com/Shivam-2WaysTech/n8n-enterprise-ai-setup-free.git
cd n8n-enterprise-ai-setup-free

# Deploy with superpowers activated
./scripts/setup.sh

# Launch the fortress
docker compose up -d

# Verify your powers
./scripts/health-check.sh
```

**Option 2: The Manual Master Route** 🎯
```bash
# 1. Prepare your environment
cp .env.example .env

# 2. Update .env file
# CRITICAL: Generate an encryption key (openssl rand -base64 32)
# CRITICAL: Set user/password for DB, Redis, and Basic Auth

# 3. Launch all systems
docker compose up -d
```

### 🛡️ Configure Nginx Proxy Manager (NPM)

1. Access NPM Admin UI: `http://localhost:81`
2. Default Login:
   - Email: `admin@example.com`
   - Password: `changeme`
3. Change credentials as prompted.
4. **Add Proxy Host**:
   - Domain: `n8n.your-domain.com` (or similar)
   - Scheme: `http`
   - Forward Hostname: `n8n`
   - Forward Port: `5678`
   - Enable "Block Common Exploits" and "Websockets Support".
   - SSL: Request a new Let's Encrypt certificate (Force SSL recommended).

---

## 📧 Email Powers Activation (SMTP)

Activate specialized communication channels for your automation agents.

### 🔥 Gmail Setup (Recommended for Heroes)

1. **Enable 2-Factor Authentication** on your Google account.
2. **Generate App Password** (Security → 2-Step Verification → App passwords).
3. **Update `.env`**:
   ```bash
   N8N_SMTP_HOST=smtp.gmail.com
   N8N_SMTP_PORT=587
   N8N_SMTP_USER=your-email@gmail.com
   N8N_SMTP_PASS=your-16-character-app-password
   N8N_SMTP_SENDER=your-email@gmail.com
   N8N_SMTP_SSL=false
   N8N_SMTP_STARTTLS=true
   ```

### ⚡ Other Providers

**Microsoft Outlook/Hotmail**:
```bash
N8N_SMTP_HOST=smtp-mail.outlook.com
N8N_SMTP_PORT=587
```

**Yahoo Mail**:
```bash
N8N_SMTP_HOST=smtp.mail.yahoo.com
N8N_SMTP_PORT=587
```

---

## ⚡ Superpower Demonstrations (Usage)

### 🎮 Basic Commands (Your Daily Arsenal)

```bash
# 🚀 Launch the fortress
docker compose up -d

# 🔍 Check your army status
docker compose ps

# 🏥 Run full health diagnostics
./scripts/health-check.sh

# 📊 Monitor your forces in real-time
docker compose logs -f

# 🛑 Graceful shutdown
docker compose down
```

### 🔄 Scaling Your Army (Worker Management)

```bash
# 📈 Scale up workers for heavy lifting
docker compose up -d --scale n8n-worker=5

# 📉 Scale down for resource conservation
docker compose up -d --scale n8n-worker=2
```

### 🏥 Health Checks & Monitoring

Inside the command center, you can monitor every pulse of your fortress:

```bash
# 🔍 Individual Service Health
docker compose exec n8n curl -f http://localhost:5678/healthz
docker compose exec postgres pg_isready -U n8n
docker compose exec redis redis-cli -a <password> ping

# 📈 Resource Usage
docker stats
```

---

## 🏛️ Command Center Blueprint (Project Structure)

```
n8n-enterprise-ai-setup-free/
├── 🏗️ docker-compose.yaml          # The master orchestration file
├── 🔧 .env                         # Your secret configuration vault
├── 📋 .env.example                 # Configuration template & guide
├── 📚 README.md                    # This epic documentation
├── 🛡️ nginx/                       # (Legacy) Nginx config
└── 🔧 scripts/                     # Automation utilities
    ├── setup.sh                    # Automated setup wizard
    └── health-check.sh             # System diagnostics tool
```

---

## 🎯 The Arsenal (Advanced Features)

### 💾 Backup & Restore (Data Protection)

```bash
# 🛡️ Create fortress backup
mkdir -p backups/$(date +%Y%m%d)

# 💾 Backup database
docker compose exec postgres pg_dump -U n8n n8n > backups/$(date +%Y%m%d)/database.sql

# 📦 Backup n8n workflows and settings
docker cp $(docker compose ps -q n8n):/home/node/.n8n backups/$(date +%Y%m%d)/n8n-data
```

### 🛡️ Security Considerations

1. **Encryption Key**: Keep your `N8N_ENCRYPTION_KEY` secure and consistent.
2. **Local Development**: This setup is optimized for local development. For production:
    - Enable **Basic Auth** (`N8N_BASIC_AUTH_ACTIVE=true`).
    - Use strong passwords for DB and Redis.
    - Use NPM to enforce HTTPS.

---

## 🆘 Emergency Protocols (Troubleshooting)

### 🚨 Critical Issues

#### 🔥 n8n Won't Start
Check logs for Beta instability:
```bash
docker compose logs n8n
```
If the beta version is unstable, you can revert to `image: n8nio/n8n:latest` in `docker-compose.yaml`.

#### 🐘 Database Connection Issues
```bash
docker compose exec postgres pg_isready -U n8n
# If needed, reset database (WARNING: destroys data)
docker compose down -v
docker compose up -d
```

#### 🌐 Port Conflicts
If port 80/443/81 are busy, find the culprit:
```bash
lsof -i :80
```
Stop the conflicting service or update `docker-compose.yaml` ports.

---

<div align="center">

### 🚀 Ready to Build the Future of Automation?

**Your fortress awaits. Deploy now and unleash your automation superpowers!**

```bash
docker compose up -d
```

**🎯 Access your automation fortress: http://localhost:5678**
*(Or your configured domain via NPM)*

---

*"With great automation comes great responsibility"* - Uncle Ben (probably)

**Built with ❤️ by [Shivam Chopra](https://github.com/Shivam-2WaysTech) | Powered by n8n | Made for Heroes**

</div>