# Деплой retail-audit-kit на Windows Server

Связанный проект: [[retail-audit-kit - корпоративный аудит розницы]].

## Целевая среда

- Windows Server 2016/2019/2022
- корпоративная сеть
- PostgreSQL on-premises
- backend на Uvicorn workers
- frontend через Vite preview или статическую раздачу

Связь: [[Windows Server deployment]].

## Общий порядок

1. Установить Python 3.11+, Node.js 18+, PostgreSQL 14+ и Git.
2. Склонировать репозиторий.
3. Создать `backend/.env` и `frontend/.env`.
4. Установить backend-зависимости.
5. Установить frontend-зависимости.
6. Выполнить Alembic-миграции.
7. Открыть порт backend.
8. Запустить `start_prod.bat` или оформить backend как Windows service.

## Firewall

```bat
netsh advfirewall firewall add rule name="CheckList API" dir=in action=allow protocol=TCP localport=8000
```

## Важные переменные

Backend:

- `DB_HOST`
- `DB_PORT`
- `DB_NAME`
- `DB_USER`
- `DB_PASSWORD`
- `USERS_admin`
- `USERS_users`
- `FRONTEND_URL_PROD`
- `APP_ENV=prod`
- `APP_HOST`
- `APP_PORT`
- `APP_THREADS`
- `PHOTO_ALLOWED_ROOTS`
- `PHOTO_DRIVE_MAPPINGS`

Frontend:

- `VITE_API_URL`

## Типичные проблемы

| Симптом | Где смотреть |
|---|---|
| Backend не подключается к БД | `backend/.env`, PostgreSQL service, firewall |
| CORS ошибка | `FRONTEND_URL_DEV`, `FRONTEND_URL_PROD`, origin браузера |
| Нет шаблонов | `python -m alembic upgrade head` |
| Фото не открываются | [[Корпоративное хранилище фото]] |
| Пользователь не входит | [[HTTP Basic Auth]] |
