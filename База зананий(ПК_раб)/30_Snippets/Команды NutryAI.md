# Команды NutryAI

Связанный проект: [[NutryAI - ИИ-дневник питания]].

Корень git-репозитория:

```powershell
cd C:\Users\kobel\NutryAI\my-app
```

## Backend dev

```powershell
cd C:\Users\kobel\NutryAI\my-app\app\backend
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python -m alembic upgrade head
$env:IS_LAMBDA = "false"
.\run_dev.ps1
```

Эквивалентный запуск uvicorn:

```powershell
cd C:\Users\kobel\NutryAI\my-app\app\backend
$env:IS_LAMBDA = "false"
.\.venv\Scripts\python.exe -m uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

API docs:

```text
http://127.0.0.1:8000/docs
```

## Alembic

Проверить heads перед новой миграцией:

```powershell
cd C:\Users\kobel\NutryAI\my-app\app\backend
python -c "from alembic.config import Config; from alembic.script import ScriptDirectory; cfg = Config('alembic.ini'); script = ScriptDirectory.from_config(cfg); print('Heads:', script.get_heads())"
```

Применить миграции:

```powershell
python -m alembic upgrade head
python -m alembic current
```

## Frontend dev

```powershell
cd C:\Users\kobel\NutryAI\my-app\app\web
npm install
npm run dev
```

Открыть:

```text
http://localhost:3000/admin/login
```

В dev `next.config.ts` проксирует `/api/*` на backend `http://127.0.0.1:8000`.

## Frontend build

```powershell
cd C:\Users\kobel\NutryAI\my-app\app\web
npm run build
npm run start
```

## Docker compose

```powershell
cd C:\Users\kobel\NutryAI\my-app\app
copy .env.example .env
# заполнить секреты

docker compose build
docker compose up -d
```

Сервисы:

- API: `http://localhost:8000`
- Web: `http://localhost:3000`

## VM deploy

Команда из `MEMORY_BANK.md`:

```bash
sudo env APP_ROOT=/home/nutriaidiary/nutriaidiary-src/app bash /home/nutriaidiary/nutriaidiary-src/app/scripts/vm_deploy_pull.sh
```

Поток: `git pull` -> `alembic upgrade head` -> `npm build` -> restart systemd.

## Частые проверки

```powershell
curl http://127.0.0.1:8000/health
curl http://127.0.0.1:8000/database/health
```

## Связи

- [[NutryAI - архитектура]]
- [[FastAPI]]
- [[Next.js App Router]]
- [[Alembic]]
- [[Docker Compose]]
