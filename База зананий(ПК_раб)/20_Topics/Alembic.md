# Alembic

Alembic управляет миграциями схемы базы данных.

## Где смотреть

- `alembic.ini`
- `alembic/env.py`
- `alembic/versions/`

## В проекте

`alembic/env.py` берет `database_url` из `get_settings()` и подключает `Base.metadata`.

Важно: модели должны быть импортированы в `env.py`, чтобы autogenerate увидел таблицы.

## Команды

```powershell
alembic revision --autogenerate -m "message"
alembic upgrade head
alembic downgrade -1
```

## Связи

- [[PostgreSQL]]
- [[SQLAlchemy 2 async]]
