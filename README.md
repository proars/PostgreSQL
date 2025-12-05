# 🐘 PostgreSQL + pgAdmin Docker Stack

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)  
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)  
![License](https://img.shields.io/badge/license-MIT-blue.svg?style=for-the-badge)

> Production-ready PostgreSQL 16 and pgAdmin 4 development environment with Docker Compose. Easy setup, secure configuration, and persistent data storage.

## ✨ Features

*   🚀 **Quick Setup** - Get PostgreSQL running in under 2 minutes
*   🔒 **Secure by Default** - Environment-based secrets management
*   💾 **Persistent Storage** - Data survives container restarts
*   🌐 **pgAdmin Included** - Web-based database management interface
*   🔄 **Auto-restart** - Containers restart automatically on failure
*   🐳 **Docker Compose** - Simple orchestration and deployment

## 🚀 Quick Start

### 1\. Clone the Repository

```
git clone https://github.com/yourusername/postgres-docker-stack.git
cd postgres-docker-stack
```

### 2\. Configure Environment

**⚠️ Important:** Edit the `.env` file and change the default passwords before starting!

```
# Copy example and edit
cp .env .env.backup  # Optional: backup original
nano .env            # Or use your preferred editor
```

**Required changes in** `**.env**`**:**

*   `POSTGRES_PASSWORD` - Change from default
*   `PGADMIN_DEFAULT_PASSWORD` - Change from default
*   `PGADMIN_DEFAULT_EMAIL` - Set your email

See Configuration section below for all available variables.

### 3\. Start the Stack

```
docker compose up -d
```

### 4\. Access Services

**Local Access (same host):**

*   PostgreSQL: `localhost:5432`
*   pgAdmin: http://localhost:5050

**Remote Access (from other hosts):**

*   PostgreSQL: `<server-ip>:5432`
*   pgAdmin: `http://<server-ip>:5050`

## ⚙️ Configuration

Edit `.env` file in the project root:

### Environment Variables

| Variable | Description | Default |
| --- | --- | --- |
| `POSTGRES_USER` | PostgreSQL superuser name | `admin` |
| `POSTGRES_PASSWORD` | PostgreSQL superuser password | `strongpassword` |
| `POSTGRES_DB` | Default database name | `mydatabase` |
| `TZ` | Timezone for containers | `America/Los_Angeles` |
| `PGADMIN_DEFAULT_EMAIL` | pgAdmin login email | `admin@example.com` |
| `PGADMIN_DEFAULT_PASSWORD` | pgAdmin login password | `strongpassword` |

> **Note:** Data is stored in Docker-managed volumes. No manual path configuration needed!

## 🔧 Services

### PostgreSQL 16

*   **Image:** `postgres:16`
*   **Port:** `5432`
*   **Features:**
    *   Latest PostgreSQL 16 stable release
    *   Automatic health checks
    *   Persistent data storage
    *   Custom timezone support

### pgAdmin 4

*   **Image:** `dpage/pgadmin4:latest`
*   **Port:** `5050` (mapped to container port 80)
*   **Features:**
    *   Modern web-based interface
    *   Query tool and visual explain
    *   Database backup and restore
    *   User management

## 💻 Usage

### View Container Logs

```
# PostgreSQL logs
docker logs postgres_db

# pgAdmin logs
docker logs pgadmin

# Follow logs in real-time
docker logs -f postgres_db
```

### Manage Services

```
# Restart all services
docker compose restart

# Restart specific service
docker compose restart postgres

# Stop all services
docker compose stop

# Start stopped services
docker compose start
```

### Backup Database

```
# Create backup
docker exec postgres_db pg_dump -U admin mydatabase > backup.sql

# Restore backup
docker exec -i postgres_db psql -U admin mydatabase < backup.sql
```

### Connect to PostgreSQL CLI

```
docker exec -it postgres_db psql -U admin -d mydatabase
```

### Stop and Remove

```
# Stop and remove containers (data persists in volumes)
docker compose down

# Stop and remove containers AND volumes (deletes all data!)
docker compose down -v
```

## 🔐 Security

### Production Deployment Checklist

*   Change all default passwords in `.env`
*   Add `.env` to `.gitignore` (already included)
*   Configure firewall rules to restrict port access
*   Use strong passwords (16+ characters)
*   Set up automated backups
*   Enable SSL/TLS for PostgreSQL connections

### Firewall Configuration

**Allow PostgreSQL from specific IP:**

```
sudo ufw allow from 192.168.1.100 to any port 5432
```

**Allow pgAdmin from local network:**

```
sudo ufw allow from 192.168.1.0/24 to any port 5050
```

## 🐛 Troubleshooting

### Container Won't Start

```
# Check container status
docker compose ps

# View detailed logs
docker compose logs

# Follow logs in real-time
docker compose logs -f
```

### Database Initialization Errors

If you see errors like "directory exists but is not empty", reset the volumes:

```
# Stop containers
docker compose down

# Remove volumes (this deletes all data!)
docker volume rm postgres_postgres-data postgres_pgadmin-data

# Start fresh
docker compose up -d
```

### pgAdmin Permission Errors

The stack uses `user: root` for pgAdmin to avoid permission issues. If problems persist:

```
# Recreate containers
docker compose up -d --force-recreate

# Or reset volumes (see above)
```

### Can't Connect from Remote Host

1.  Verify firewall allows connections
2.  Check Docker port binding: `docker port postgres_db`
3.  Test connection: `telnet <server-ip> 5432`
4.  Ensure PostgreSQL accepts remote connections (enabled by default)

### Check Volume Status

```
# List volumes
docker volume ls

# Inspect volume
docker volume inspect postgres_postgres-data
```

## 📄 License

MIT License - feel free to use this project for personal or commercial purposes.

## ⭐ Show Your Support

Give a ⭐️ if this project helped you!

---

**Made with ❤️ for the PostgreSQL community**