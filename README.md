## 👋 Welcome to komodo 🚀

A web-based, hybrid server management platform

## 📋 Description

Komodo is a self-hosted infrastructure management tool. The Core service holds
the web UI and API, Periphery is the agent that runs on each managed host, and
MongoDB stores the state. Stacks are defined as TOML and deployed through
Periphery.

## 🚀 Services

- **app**: ghcr.io/mbecker20/komodo:latest
- **periphery**: ghcr.io/mbecker20/periphery:latest

### Infrastructure Components

- **db**: MongoDB database

## 📦 Installation

### Option 1: Quick Install

```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/komodo/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone

```bash
git clone "https://github.com/composemgr/komodo" ~/.local/srv/docker/komodo
cd ~/.local/srv/docker/komodo
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr

```bash
composemgr install komodo
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
APP_ORG_NAME=Komodo
APP_SECRET_KEY=replace-with-a-random-16-byte-hex-string
RPC_SECRET=replace-with-a-random-webhook-secret
DB_USER_NAME=komodo
DB_ADMIN_PASS=replace-with-a-random-password
```

See `docker-compose.yaml` for complete list of configurable options.

### Environment Files

`default.env.sample` holds the full set of variables and is copied to `.env`
before starting the stack:

```bash
cp default.env .env
docker compose --env-file .env up -d
```

`app.env.sample` holds the app-specific overrides and is copied to `app.env`:

```bash
cp app.env.sample app.env
```

`composemgr up` applies `app.env` and `default.env` automatically. Raw
`docker compose` commands need `--env-file .env` to pick up the same values.

### Reverse Proxy

Three standalone compose files ship in this repo:

| File | Use |
|------|-----|
| `docker-compose.yaml` | Default, published on `172.17.0.1:64226` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

The proxy variants publish no host ports. Only the Komodo Core service is
reachable through the proxy network. The Periphery agent and MongoDB service
stay internal to the project network in every variant.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:64226

Only Core is published. Periphery is reached by Core over the project network
and is never exposed to the host.

## 📂 Volumes

- `./volumes/data/komodo/repo-syncs` - Git repository sync checkouts
- `./volumes/data/komodo/repo-cache` - Repository cache
- `./volumes/config/komodo/repos` - Periphery configuration
- `./volumes/data/komodo/repos` - Managed repository files
- `./volumes/data/komodo/stacks` - Deployed stack files
- `./var/run/docker.sock` - Docker socket, mounted so Periphery can deploy stacks
- `./proc:/proc` - Host procfs, mounted so Periphery can read host metrics
- `./volumes/db/mongo/komodo` - MongoDB database data

## 🔐 Security

- Change all default passwords before deploying to production
- Use strong secrets for all authentication tokens
- Configure HTTPS using a reverse proxy (nginx, traefik, caddy)
- Regularly update Docker images for security patches
- Backup your data regularly

Periphery mounts the Docker socket and the host `proc` filesystem, so it has
broad control over the host. Both grants are required for Komodo to deploy
stacks and report resource usage; keep the stack on a trusted network and
change `APP_SECRET_KEY` and `RPC_SECRET` before first use.

## 🔍 Logging

```shell
docker compose --env-file .env logs -f app
docker compose --env-file .env logs -f periphery
```

## 🛠️ Management

```bash
# Start services
docker compose --env-file .env up -d

# Stop services
docker compose --env-file .env down

# Update to latest images
docker compose --env-file .env pull && docker compose --env-file .env up -d

# View logs
docker compose --env-file .env logs -f

# Restart services
docker compose --env-file .env restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
