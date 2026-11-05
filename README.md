# Compose stacks

Production-ready Docker Compose stacks for popular self-hosted apps, each with **automatic HTTPS via Caddy**, pinned images, health checks, persistent volumes and a `.env.example`.

| Stack | App | One-click alternative |
|---|---|---|
| [`n8n/`](n8n) | n8n workflow automation + Postgres | [![Deploy to VeloxaHost](https://veloxahost.com/badges/deploy-to-veloxahost.svg)](https://veloxahost.com/apps/n8n?utm_source=deploy-button&utm_medium=referral&utm_campaign=n8n) |
| [`uptime-kuma/`](uptime-kuma) | Uptime Kuma status monitoring | [![Deploy to VeloxaHost](https://veloxahost.com/badges/deploy-to-veloxahost.svg)](https://veloxahost.com/apps/uptime-kuma?utm_source=deploy-button&utm_medium=referral&utm_campaign=uptime-kuma) |
| [`ghost/`](ghost) | Ghost blog + MySQL 8 | [![Deploy to VeloxaHost](https://veloxahost.com/badges/deploy-to-veloxahost.svg)](https://veloxahost.com/apps/ghost?utm_source=deploy-button&utm_medium=referral&utm_campaign=ghost) |
| [`vaultwarden/`](vaultwarden) | Vaultwarden (Bitwarden-compatible) password manager | [![Deploy to VeloxaHost](https://veloxahost.com/badges/deploy-to-veloxahost.svg)](https://veloxahost.com/apps/vaultwarden?utm_source=deploy-button&utm_medium=referral&utm_campaign=vaultwarden) |

## Use a stack

Requirements: a Linux server with Docker + Compose v2, a domain pointing at the server (A record), ports 80 and 443 open.

```bash
git clone https://github.com/veloxahost/compose-stacks.git
cd compose-stacks/n8n
cp .env.example .env && nano .env      # set DOMAIN and passwords
docker compose up -d
docker compose logs -f
```

Caddy obtains and renews a Let's Encrypt certificate for `DOMAIN` on first start.

## Conventions

- Only Caddy publishes ports (80/443). Apps listen on the internal network only — Docker-published ports bypass UFW, so we avoid them.
- Images are pinned to a major version where upstream publishes one (Vaultwarden only offers `latest-alpine`; pin a release tag if you prefer). Update with `docker compose pull && docker compose up -d` after reading the app's release notes.
- Data lives in named volumes. Back up with `docker run --rm -v <volume>:/v -v $PWD:/b alpine tar czf /b/<volume>.tgz -C /v .`
- Secrets go in `.env` (`chmod 600 .env`), never in `compose.yaml`.

## Hosting

Any VPS works. On [VeloxaHost](https://veloxahost.com/vps/docker-compose?utm_source=github&utm_medium=referral&utm_campaign=compose-stacks), servers are Docker-ready, bill hourly from $4/mo and take snapshots before upgrades in one click.

## License

MIT
