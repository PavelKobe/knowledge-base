# Диагностика PostgreSQL и health-db

Связанный проект: [[shop-backend-course - учебный backend-стек]].

## Симптом

Endpoint:

```text
/health/db
```

должен вернуть:

```json
{"db": 1}
```

Если вместо этого 500, проблема почти всегда в подключении к [[PostgreSQL]].

## Проверить Docker

```powershell
docker compose ps
docker compose logs -f db
```

## Проверить порт 5432

```powershell
Get-NetTCPConnection -LocalPort 5432 -State Listen | Select-Object OwningProcess
Get-Service postgresql*
```

Если локальный PostgreSQL занял порт 5432, Docker-контейнер или приложение могут подключаться не туда.

## Проверить строку подключения

В `.env` или `app/core/config.py`:

```text
DATABASE_URL=postgresql+asyncpg://shop:shop@localhost:5432/shop
```

## Важно

После изменения `.env` нужно перезапустить uvicorn вручную. `--reload` следит за `.py`, но не всегда корректно перечитывает `.env`.

## Связи

- [[PostgreSQL]]
- [[SQLAlchemy 2 async]]
- [[Docker Compose]]
- [[Команды shop-backend-course]]
