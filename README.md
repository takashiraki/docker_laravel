# Docker Laravel Environment

A Docker-based development environment for Laravel applications with SSL support via Let's Encrypt and nginx-proxy integration.

## Features

- PHP container configured for Laravel
- Docker Compose setup with multiple network support
- SSL certificate management with Let's Encrypt
- Vendor volume for optimized dependency management
- Integration with external infrastructure networks (proxy and database)

## Prerequisites

- Docker
- Docker Compose
- External networks must be created beforehand:
  - `my_infra_network` (for database and other infrastructure)
  - `my_proxy_network` (for nginx-proxy)

## Setup

1. Clone this repository:
   ```bash
   git clone <repository-url>
   cd docker_laravel
   ```

2. Copy the environment file and configure it:
   ```bash
   cp .env.example .env
   ```

3. Configure the `.env` file with your settings:
   ```env
   CONTAINER_NAME=your_container_name
   REPOSITORY=src
   DOCKER_PATH=Infra/php

   VIRTUAL_HOST=your-domain.com
   LETSENCRYPT_HOST=your-domain.com
   LETSENCRYPT_EMAIL=your-email@example.com
   LETSENCRYPT_TEST=true  # Set to false for production
   CERT_NAME=default
   TZ=Asia/Tokyo  # or your timezone
   ```

4. Place your Laravel application in the directory specified by `REPOSITORY` (default: `src`)

5. Start the containers:
   ```bash
   docker-compose up -d
   ```

## Directory Structure

```
.
├── Infra/
│   └── php/          # PHP Docker configuration
├── src/              # Laravel application (configurable via REPOSITORY)
├── .env.example      # Environment variables template
├── docker-compose.yml
└── README.md
```

## Environment Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `CONTAINER_NAME` | Name of the PHP container | `my_laravel_app` |
| `REPOSITORY` | Path to Laravel application | `src` |
| `DOCKER_PATH` | Path to Docker configuration | `Infra/php` |
| `VIRTUAL_HOST` | Domain for the application | `example.com` |
| `LETSENCRYPT_HOST` | Domain for SSL certificate | `example.com` |
| `LETSENCRYPT_EMAIL` | Email for Let's Encrypt | `admin@example.com` |
| `LETSENCRYPT_TEST` | Use Let's Encrypt staging (true/false) | `true` |
| `CERT_NAME` | Certificate name | `default` |
| `TZ` | Timezone | `Asia/Tokyo` |

## Networks

This setup uses three networks:

- `my_laravel_network` - Internal network for Laravel services
- `my_infra_network` - External network for infrastructure (database, cache, etc.)
- `my_proxy_network` - External network for nginx-proxy

Make sure the external networks exist before starting the containers.

## Volumes

- `vendor-store` - Persistent volume for Composer dependencies to improve performance

## Usage

### Start containers
```bash
docker-compose up -d
```

### Stop containers
```bash
docker-compose down
```

### View logs
```bash
docker-compose logs -f php
```

### Access the container
```bash
docker-compose exec php bash
```

### Install Laravel dependencies
```bash
docker-compose exec php composer install
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
