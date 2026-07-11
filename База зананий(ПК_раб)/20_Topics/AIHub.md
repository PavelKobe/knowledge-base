# AIHub

AIHub в NutryAI - backend proxy для текстовой/мультимодальной генерации и генерации изображений через OpenAI-compatible AI provider, сейчас через OpenRouter-like base URL.

## Где используется

- [[NutryAI - ИИ-дневник питания]]

## Файлы проекта

- `app/backend/routers/aihub.py`
- `app/backend/services/aihub.py`
- `app/backend/schemas/aihub.py`
- `app/web/src/app/api/v1/aihub/[...path]/route.ts`

## API

- `POST /api/v1/aihub/gentxt` - text/multimodal generation, optional SSE streaming.
- `POST /api/v1/aihub/genimg` - text-to-image / image-to-image.

## Особенность

AI запросы завязаны на подписку: dependency проверяет лимит, списывает daily quota, а при полном сбое доставки возвращает запрос в лимит.

## Env

```text
APP_AI_BASE_URL=https://openrouter.ai/api/v1
APP_AI_KEY=...
APP_AI_PROXY=...
```

## Связи

- [[OpenRouter]]
- [[NutryAI - данные и API]]
