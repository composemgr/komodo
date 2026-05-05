## 👋 Welcome to komodo 🚀

komodo - Self-hosted Docker Compose deployment

## 📋 Description

Komodo is a containerized service deployed using Docker Compose. This setup provides a complete, production-ready deployment with proper security defaults, logging, and configuration management.

## 📦 Installation

### Using curl
```shell
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/komodo/main/docker-compose.yaml" -o compose.yml
```

### Using git
```shell
git clone "https://github.com/composemgr/komodo" ~/.local/srv/docker/komodo
cd ~/.local/srv/docker/komodo
docker compose up -d
```

### Using composemgr
```shell
composemgr install komodo
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
BASE_HOST_NAME=${HOSTNAME}
BASE_DOMAIN_NAME=
```

## 🌐 Access

- **Web Interface**: http://172.17.0.1:60236

## 📂 Volumes

- `./volumes/config/komodo` - Configuration files
- `./volumes/data/komodo` - Application data

## 🔐 Security

- Change default passwords after first login
- Use HTTPS via reverse proxy in production
- Configure authentication as needed

## 🔍 Logging

```shell
docker compose logs -f
```

## 🛠️ Management

### Start services
```shell
docker compose up -d
```

### Stop services
```shell
docker compose down
```

### Update images
```shell
docker compose pull && docker compose up -d
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+
- Sufficient disk space for data and logs

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
