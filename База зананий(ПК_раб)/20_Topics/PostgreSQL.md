# PostgreSQL

PostgreSQL - основная реляционная база проекта [[shop-backend-course - учебный backend-стек]].

## Где смотреть

- `docker-compose.yml`
- `DB.md`
- `app/core/config.py`
- `app/core/db.py`

## Параметры локальной базы

```text
Host: localhost
Port: 5432
Database: shop
User: shop
Password: shop
```

Строка подключения приложения:

```text
postgresql+asyncpg://shop:shop@localhost:5432/shop
```

## Связи

- [[Docker Compose]]
- [[SQLAlchemy 2 async]]
- [[Alembic]]
- [[Диагностика PostgreSQL и health-db]]
