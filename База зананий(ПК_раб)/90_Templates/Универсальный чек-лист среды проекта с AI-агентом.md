# Универсальный чек-лист среды проекта с AI-агентом

Шаблон для инициализации нового проекта: цель, контракты, среда, права, хуки, команды и проверки. Собрано из трех методичек по agent/harness workflow и адаптировано под стек базы: [[FastAPI]], [[Pydantic v2]], [[PostgreSQL]], [[SQLAlchemy 2 async]], [[Alembic]], [[Repository Service Pattern]], [[Docker Compose]], [[Pytest]], [[Ruff Mypy pre-commit]], [[React]], [[Vite]], [[Redis]], [[Celery]], [[RabbitMQ]], [[Nginx Prometheus]].

## 0. Главное правило

Среда готова не тогда, когда агент "понял договоренности", а когда свежий клон поднимается одной командой, проверки запускаются без подтверждений, опасные действия блокируются правами/хуками, а настоящих секретов в окружении агента физически нет.

## 1. Артефакты до кода

- [ ] `docs/idea.md`: проблема, пользователь, тонкий сквозной сценарий, baseline, целевая метрика, kill-критерий.
- [ ] `docs/spec.md`: что строим, основные сценарии, ограничения, anti-scope.
- [ ] `docs/api.md`: endpoint, input, output, ошибки, пример.
- [ ] `docs/decisions/001-*.md`: контекст, решение, последствия.
- [ ] Один сквозной сценарий готов к превращению в e2e-тест.
- [ ] В `README.md` есть команды: `dev`, `test`, `lint`, `typecheck`, `check`, `migrate`.

```text
docs/
  idea.md
  spec.md
  api.md
  decisions/
app/ или backend/
tests/
scripts/
evals/          # только если ИИ есть внутри продукта
.env.example
README.md
```

## 2. Базовый стек по умолчанию

Backend:

- [ ] [[FastAPI]] как HTTP/API слой.
- [ ] [[Pydantic v2]] для схем и настроек.
- [ ] [[PostgreSQL]] как основная БД.
- [ ] [[SQLAlchemy 2 async]] + `asyncpg` как ORM/runtime доступ к БД.
- [ ] [[Alembic]] для миграций. Схема БД меняется только миграциями.
- [ ] [[Repository Service Pattern]]: router -> service -> repository -> database.
- [ ] [[Pytest]] для unit/integration/e2e проверок.
- [ ] [[Ruff Mypy pre-commit]] для форматирования, линтинга и типов.

Frontend, если нужен интерфейс:

- [ ] [[React]] + [[Vite]].
- [ ] API-клиент отделен от компонентов: `frontend/src/api.*` или `web/src/api.*`.
- [ ] Есть команды `lint`, `typecheck`, `build`.

Инфраструктура:

- [ ] [[Docker Compose]] поднимает PostgreSQL и локальные сервисы.
- [ ] [[Redis]] подключается только если нужен кеш/очереди.
- [ ] [[Celery]] + [[RabbitMQ]] или Redis broker подключаются только при реальных фоновых задачах.
- [ ] [[Nginx Prometheus]] закладываются для production-наблюдаемости.

AI внутри продукта:

- [ ] `evals/` есть только если ИИ является частью продукта, а не только способом разработки.
- [ ] Есть golden set: 30-50 реальных примеров с эталоном.
- [ ] В CI есть блокирующий порог качества evals.

## 3. Команды проекта

В каждом проекте должен быть один стабильный слой команд: `Makefile`, `Taskfile.yml`, `justfile` или `scripts/*.ps1`.

```powershell
make dev        # поднять локальную среду
make test       # все тесты
make lint       # ruff check + frontend lint, если есть
make format     # ruff format + frontend format, если есть
make typecheck  # mypy + frontend typecheck, если есть
make check      # lint + typecheck + test
make migrate    # alembic upgrade head
make makemigration name="..."  # alembic revision --autogenerate
make seed       # стартовые данные
make logs       # docker compose logs -f
make down       # остановить локальную среду
```

Если `make` неудобен на Windows, сделать зеркала:

```powershell
.\scripts\dev.ps1
.\scripts\test.ps1
.\scripts\check.ps1
.\scripts\migrate.ps1
```

## 4. Файл правил для агента

Создать `AGENTS.md`, `CLAUDE.md` или аналог инструмента. Держать коротким: команды, архитектура, правила, запреты.

```markdown
# <project-name>

## Commands

- `make dev` - start local environment
- `make test` - run tests
- `make lint` - run linters
- `make typecheck` - run type checks
- `make check` - lint + typecheck + tests
- `make migrate` - apply migrations

## Architecture

- Backend: FastAPI + Pydantic v2.
- Database: PostgreSQL via SQLAlchemy 2 async / asyncpg.
- Migrations: Alembic only. Never edit DB schema manually.
- Code shape: API router -> service -> repository -> database.
- Frontend, if present: React + Vite.

## Rules

- Change database schema only through Alembic migrations.
- Do not log secrets, tokens, passwords, personal data or raw production payloads.
- Keep API contracts in sync with `docs/api.md` and tests.
- Before finishing a task, run `make check` or explain why it cannot run.
- One task = one focused diff. Avoid unrelated refactors.

## Do Not

- Do not edit `.env`, secrets or production config.
- Do not run destructive database commands without explicit approval.
- Do not change public API formats without updating `docs/spec.md`, `docs/api.md` and tests.
- Do not push, deploy or merge without explicit approval.
```

## 5. Разрешения

Проектировать с конца: `deny` -> `ask` -> `allow`.

Никогда:

- [ ] `git push`, merge, deploy.
- [ ] Удаление директорий и массовые удаления.
- [ ] Чтение `.env`, секретов, production dump, ключей.
- [ ] Команды к production БД.
- [ ] Изменение CI/CD, nginx, systemd, docker production config без подтверждения.

С вопросом:

- [ ] `git commit`.
- [ ] Установка новых зависимостей.
- [ ] `alembic revision`, `alembic upgrade`, `alembic downgrade`.
- [ ] Изменение Docker/CI/deploy-конфигов.
- [ ] Запуск долгих тестов/evals.

Молча:

- [ ] Чтение проекта.
- [ ] Правки в `app/**`, `backend/**`, `frontend/**`, `web/**`, `tests/**`, `docs/**`.
- [ ] `make test`, `make lint`, `make typecheck`, `make check`.
- [ ] `docker compose ps`, `docker compose logs`.

Пример структуры для Claude Code:

```json
{
  "permissions": {
    "defaultMode": "acceptEdits",
    "allow": [
      "Bash(make test)",
      "Bash(make lint)",
      "Bash(make typecheck)",
      "Bash(make check)",
      "Bash(docker compose ps)",
      "Bash(docker compose logs:*)",
      "Edit(app/**)",
      "Edit(backend/**)",
      "Edit(frontend/**)",
      "Edit(web/**)",
      "Edit(tests/**)",
      "Edit(docs/**)"
    ],
    "ask": [
      "Bash(git commit:*)",
      "Bash(pip install:*)",
      "Bash(poetry add:*)",
      "Bash(uv add:*)",
      "Bash(npm install:*)",
      "Bash(alembic revision:*)",
      "Bash(alembic upgrade:*)",
      "Bash(alembic downgrade:*)",
      "Edit(.github/**)",
      "Edit(docker-compose*.yml)",
      "Edit(Dockerfile)",
      "Edit(nginx/**)"
    ],
    "deny": [
      "Bash(git push:*)",
      "Bash(rm -rf:*)",
      "Bash(del /s:*)",
      "Bash(Remove-Item -Recurse:*)",
      "Read(.env)",
      "Read(.env.*)",
      "Read(secrets/**)",
      "Read(*prod*)"
    ]
  }
}
```

Важно: запрет на чтение `.env` не полноценная защита, если агент может запустить скрипт и прочитать файл сам. Настоящая защита: в окружении агента нет настоящих секретов, только `.env.example` и dev-заглушки.

## 6. Хуки, которые должны быть в каждом проекте

Минимальный набор:

- [ ] PreToolUse / pre-command guard: блокирует опасные команды.
- [ ] PostToolUse / after-edit: форматирует измененные файлы.
- [ ] PostToolUse / after-edit: запускает быстрый lint/typecheck.
- [ ] Git `pre-commit`: не дает закоммитить код без проверок.
- [ ] Git `pre-push`: блокирует push, если тесты/evals красные.
- [ ] SessionStart/bootstrap hook: показывает агенту команды проекта и ограничения.
- [ ] Stop hook: напоминает запустить `make check`, если были правки.

Пример `.claude/settings.json`:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "python .claude/hooks/guard.py",
            "timeout": 10
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "python .claude/hooks/after_edit.py",
            "timeout": 120
          }
        ]
      }
    ],
    "Stop": [
      {
        "matcher": "*",
        "hooks": [
          {
            "type": "command",
            "command": "python .claude/hooks/stop_check.py",
            "timeout": 20
          }
        ]
      }
    ]
  }
}
```

Пример `.claude/hooks/guard.py`:

```python
import os
import re
import sys

command = os.environ.get("CLAUDE_TOOL_INPUT", "")

deny_patterns = [
    r"\bgit\s+push\b",
    r"\brm\s+-rf\b",
    r"\bRemove-Item\b.*\b-Recurse\b",
    r"\balembic\s+downgrade\b",
    r"\bDROP\s+DATABASE\b",
    r"\bDROP\s+SCHEMA\b",
    r"\bprod\b",
]

for pattern in deny_patterns:
    if re.search(pattern, command, flags=re.IGNORECASE):
        print(f"Blocked by project guard: {pattern}")
        sys.exit(2)

sys.exit(0)
```

Пример `.claude/hooks/after_edit.py`:

```python
import subprocess
import sys

checks = [
    ["make", "lint"],
    ["make", "typecheck"],
]

for command in checks:
    result = subprocess.run(command, text=True, timeout=120)
    if result.returncode != 0:
        sys.exit(result.returncode)
```

Git `pre-commit`:

```sh
#!/bin/sh
make lint && make typecheck || exit 1
```

Git `pre-push`:

```sh
#!/bin/sh
make check || exit 1
```

`.pre-commit-config.yaml` для Python:

```yaml
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.5.0
    hooks:
      - id: ruff
        args: [--fix]
      - id: ruff-format
```

## 7. Контейнер и секреты

- [ ] `docker-compose.yml` поднимает dev-инфраструктуру: PostgreSQL, Redis/RabbitMQ при необходимости.
- [ ] `.env.example` содержит только dev-заглушки.
- [ ] Настоящий `.env` не читается агентом и не хранится в репозитории.
- [ ] Для агента используется dev-only БД.
- [ ] Read-only доступ к внешним MCP/БД, если агенту нужно только смотреть схему.
- [ ] Bypass/разрешить-все режим допустим только в контейнере без секретов и без доступа к production сети.

```yaml
services:
  db:
    image: postgres:16
    environment:
      POSTGRES_DB: app
      POSTGRES_USER: app
      POSTGRES_PASSWORD: app
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7
    ports:
      - "6379:6379"

volumes:
  postgres_data:
```

## 8. Каркас проекта до первой фичи

- [ ] Создано приложение `app/main.py` или `backend/main.py`.
- [ ] Есть `/health` и `/version`.
- [ ] Есть настройки через `pydantic-settings`.
- [ ] Есть подключение к БД и health-check БД.
- [ ] Есть первая Alembic migration.
- [ ] Есть пустые слои `api/`, `services/`, `repositories/`, `models/`, `schemas/`.
- [ ] Есть `tests/test_e2e.py` со сквозным красным тестом.
- [ ] Все команды из раздела 3 существуют, даже если часть пока заглушки.

```text
app/
  main.py
  core/
    config.py
    db.py
  api/
    router.py
    deps.py
  models/
  schemas/
  repositories/
  services/
tests/
  test_health.py
  test_e2e.py
alembic/
```

## 9. CI/CD

- [ ] CI использует те же команды, что локально.
- [ ] Порядок: `lint` -> `typecheck` -> `test` -> `evals`, если есть -> `build`.
- [ ] Docker image собирается после проверок.
- [ ] Миграции вынесены в отдельный контролируемый шаг.
- [ ] Merge/deploy остается человеческим решением.

```yaml
steps:
  - run: make lint
  - run: make typecheck
  - run: make test
  - run: make evals
  - run: docker build -t app:${GITHUB_SHA} .
```

## 10. Наблюдаемость

Обычный backend:

- [ ] Структурированные логи.
- [ ] `/health`, `/health/db`, `/version`.
- [ ] Метрики latency, error rate, DB errors.
- [ ] Production reverse proxy: nginx + TLS.

AI-продукт:

- [ ] Логируется `trace_id` на весь агентный/LLM цикл.
- [ ] Логируются модель, latency, tokens in/out, стоимость, tool calls, outcome.
- [ ] Есть алерты на рост стоимости, p95 latency, error rate, долю эскалаций.
- [ ] Провалы из production добавляются в `evals/cases.jsonl`.

## 11. Проверка готовности среды

- [ ] Свежий клон поднимается командой `make dev`.
- [ ] `make check` проходит или ожидаемо падает только на красном e2e-тесте.
- [ ] `git push` блокируется.
- [ ] Удаление директории блокируется.
- [ ] Агент не видит настоящие секреты.
- [ ] После правки файла автоматически срабатывает формат/линт.
- [ ] В `docs/` есть идея, спека, контракты и ADR.
- [ ] В README есть команды и краткая архитектура.

## 12. Стартовый промпт для нового проекта

```text
Ты работаешь в новом проекте.

1. Прочитай README.md, docs/idea.md, docs/spec.md, docs/api.md и AGENTS.md/CLAUDE.md.
2. Проверь, какие команды доступны: dev, test, lint, typecheck, check, migrate.
3. Не пиши код сразу.
4. Сначала составь plan.md: что нужно сделать, какие файлы изменить, какие тесты добавить.
5. После подтверждения напиши падающий тест.
6. Потом реализуй код до зеленого теста.
7. В конце покажи diff и результат make check.
```

## Связи

- [[FastAPI]]
- [[Pydantic v2]]
- [[PostgreSQL]]
- [[SQLAlchemy 2 async]]
- [[Alembic]]
- [[Repository Service Pattern]]
- [[Docker Compose]]
- [[Pytest]]
- [[Ruff Mypy pre-commit]]
- [[React]]
- [[Vite]]
- [[Nginx Prometheus]]
