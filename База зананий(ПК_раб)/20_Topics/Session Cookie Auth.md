# Session Cookie Auth

Session Cookie Auth - авторизация web-кабинета через cookie-сессию.

## Где используется

- [[VoiceScreen - голосовой AI-скрининг]]

## Файлы проекта

- `app/main.py`
- `app/api/auth.py`
- `web/src/auth/AuthProvider.tsx`
- `web/src/auth/RequireAuth.tsx`

## Как устроено

FastAPI подключает `SessionMiddleware`, если задан `SECRET_KEY`.

Настройки:

- `SECRET_KEY`
- `COOKIE_DOMAIN`
- `COOKIE_SECURE`
- `CORS_ORIGINS`

Endpoints:

- `POST /api/v1/auth/login`
- `POST /api/v1/auth/logout`
- `GET /api/v1/auth/me`
