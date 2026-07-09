# Docker Compose

Docker Compose поднимает инфраструктуру проекта одной командой.

## В проекте

Сейчас `docker-compose.yml` поднимает PostgreSQL 16.

```powershell
docker compose up -d
docker compose ps
docker compose logs -f db
```

## Связи

- [[PostgreSQL]]
- [[Nginx Prometheus]]
- [[Redis]]
- [[RabbitMQ]]
