# asyncpg

asyncpg - асинхронный PostgreSQL-драйвер.

## Где используется

В проекте [[retail-audit-kit - корпоративный аудит розницы]] runtime-доступ к базе сделан через `asyncpg`, без SQLAlchemy ORM.

Файл:

```text
backend/database.py
```

## Основные идеи

- `asyncpg.create_pool` создает пул соединений;
- FastAPI dependency `get_db` выдает соединение из пула;
- SQL-запросы выполняются напрямую через `conn.fetch`, `conn.fetchrow`, `conn.fetchval`, `conn.execute`.

## Почему это важно

Этот проект отличается от [[shop-backend-course - учебный backend-стек]], где используется [[SQLAlchemy 2 async]]. Здесь слой данных проще и ближе к ручному SQL.

## Связи

- [[PostgreSQL]]
- [[FastAPI]]
- [[Alembic]]
