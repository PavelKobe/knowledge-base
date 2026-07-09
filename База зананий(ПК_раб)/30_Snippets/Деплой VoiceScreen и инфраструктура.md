# Деплой VoiceScreen и инфраструктура

Связанный проект: [[VoiceScreen - голосовой AI-скрининг]].

## Целевая среда

- Yandex Cloud VM;
- Ubuntu 24.04;
- Docker Compose;
- домен `voxscreen.ru`;
- web-кабинет `app.voxscreen.ru`;
- nginx + TLS перед API;
- PostgreSQL + Redis + API + Celery worker + Telegram bot.

## Docker Compose

`docker-compose.yml` поднимает:

- `postgres` - PostgreSQL 15;
- `redis` - Redis 7;
- `api` - FastAPI на 8000;
- `worker` - Celery worker;
- `bot` - Telegram bot.

## Внешние сервисы

- [[OpenRouter]] - LLM gateway;
- [[Yandex SpeechKit]] - STT/TTS;
- [[Voximplant]] - телефония и VoxEngine;
- [[Yandex Object Storage]] - записи звонков;
- [[Telegram Bot]] - интерфейс HR;
- Yandex 360 SMTP - email HR.

## Nginx

Конфиг есть в:

```text
docs/nginx/app.voxscreen.ru.conf
```

Nginx принимает HTTPS, проксирует API/WebSocket и отдает web SPA.

## Риски деплоя

- WebSocket должен быть публично доступен для VoxEngine.
- Нельзя отдавать клиенту прямой Voximplant record_url с API key.
- Записи лучше отдавать через presigned URL из [[Yandex Object Storage]].
- Для звонков нужен rate-limit и контроль временного окна.
- Для персональных данных кандидатов важны ограничения 152-ФЗ.
