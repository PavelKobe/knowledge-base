# Эксплуатация NutryAI

Связанный проект: [[NutryAI - ИИ-дневник питания]].

## Production контур

По текущим файлам проект ориентирован на домены:

- frontend: `https://nutriaidiary.com`
- API: `https://api.nutriaidiary.com`
- admin: `https://admin.nutriaidiary.com`

Docker compose поднимает:

- `api` - FastAPI backend на `8000`;
- `web` - Next.js на `3000`, с `INTERNAL_API_BASE_URL=http://api:8000`.

TLS и публичный routing предполагаются через nginx/Caddy/ALB на VM.

## Критичные сервисы

| Сервис | За что отвечает | Env |
|---|---|---|
| PostgreSQL | все пользователи, дневники, подписки, платежи | `DATABASE_URL` |
| JWT auth | сессии пользователей | `JWT_SECRET_KEY` |
| OpenRouter / AIHub | AI chat, insights, images | `APP_AI_BASE_URL`, `APP_AI_KEY`, `APP_AI_PROXY` |
| YooKassa | Premium/coaching payments | `YOOKASSA_SHOP_ID`, `YOOKASSA_SECRET_KEY`, `YOOKASSA_RETURN_URL` |
| Object Storage service | presigned upload/download для файлов | `OSS_SERVICE_URL`, `OSS_API_KEY` |
| Telegram bot | bot-интерфейс | `TELEGRAM_BOT_TOKEN` |
| Resend | email verification | `RESEND_API_KEY`, `RESEND_FROM_EMAIL` |
| Web Push | уведомления | VAPID/env в push-service |
| Sentry | мониторинг backend/frontend | `SENTRY_DSN`, Next Sentry config |

## Деплой на VM

Команда:

```bash
sudo env APP_ROOT=/home/nutriaidiary/nutriaidiary-src/app bash /home/nutriaidiary/nutriaidiary-src/app/scripts/vm_deploy_pull.sh
```

Ожидаемый порядок:

1. `git pull`
2. `alembic upgrade head`
3. `npm build`
4. restart systemd services

## Health checks

```bash
curl http://127.0.0.1:8000/health
curl http://127.0.0.1:8000/database/health
```

Если deploy script пишет, что backend не отвечает, в `MEMORY_BANK.md` отмечен возможный race condition: backend стартует около 10 секунд, нужно проверить вручную health endpoint.

## Alembic правила

- Перед миграцией проверять heads.
- Не допускать `Multiple head revisions`.
- `down_revision` должен ссылаться на последний head.
- Для индексов использовать `Index(...)`.
- Не класть dict внутрь `__table_args__`.

## Barcode / products gotchas

Порядок поиска штрихкода:

1. локальный кеш `products`;
2. OpenFoodFacts;
3. FatSecret;
4. 404 -> ручной ввод на фронте.

Для FatSecret нужны:

```text
FATSECRET_CLIENT_ID=...
FATSECRET_CLIENT_SECRET=...
```

Без этих переменных FatSecret пропускается.

## AI provider gotchas

В `core/config.py` есть комментарий: Gemini недоступен текущему ключу через OpenRouter из-за блокировки Google; для микронутриентов используется `openai/gpt-4o-mini`.

`APP_AI_PROXY` нужен только для запросов к AI-провайдеру, если Cloudflare/OpenRouter блокирует IP сервера.

## Security checklist

- Не коммитить `.env`, `.env.local`, `.env_development`, `.venv`, реальные ключи.
- `ENVIRONMENT=prod` обязательно на production, чтобы локальный `.env_development` не подхватывался.
- Проверять CORS origins при добавлении новых доменов.
- YooKassa webhook дополнительно фетчит платеж из API - это важная защита от доверия сырому телу.
- Перед публичным push проверить историю на утечки секретов.
