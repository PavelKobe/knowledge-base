# OpenRouter

OpenRouter используется как LLM gateway.

## Где используется

- [[VoiceScreen - голосовой AI-скрининг]]

## Файлы проекта

- `app/core/llm.py`
- `app/core/scoring.py`
- `app/core/prompts/`

## Настройки

По умолчанию модель:

```text
openai/gpt-4o-mini
```

Переменные:

- `OPENROUTER_API_KEY`
- `OPENROUTER_BASE_URL`
- `OPENROUTER_MODEL`

В VoiceScreen LLM используется для финальной оценки, reasoning, summary и структурирования ответов.
