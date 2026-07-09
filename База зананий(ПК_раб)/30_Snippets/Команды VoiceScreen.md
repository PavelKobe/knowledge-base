# Команды VoiceScreen

Связанный проект: [[VoiceScreen - голосовой AI-скрининг]].

## Клонирование

```powershell
git clone https://github.com/PavelKobe/VoiceScreen.git
cd VoiceScreen
```

## Установка backend

```bash
python -m venv .venv
source .venv/bin/activate
make install
```

Windows-вариант:

```powershell
.\.venv\Scripts\Activate.ps1
pip install -e ".[dev]"
pre-commit install
```

## Настройки

```bash
cp .env.example .env
```

Минимально нужны `DATABASE_URL`, `REDIS_URL`, `OPENROUTER_API_KEY`, `TELEGRAM_BOT_TOKEN`. Для настоящих звонков нужны `YANDEX_*`, `VOXIMPLANT_*`, `PUBLIC_WS_URL`, `WS_AUTH_TOKEN`.

## База и Redis

```bash
make db-up
make db-migrate
```

## Запуск разработки

В отдельных терминалах:

```bash
make dev
make worker
make bot
```

API:

```text
http://localhost:8000
http://localhost:8000/docs
http://localhost:8000/health
```

## Запуск всего через Docker

```bash
docker compose up --build
```

Миграции внутри контейнера:

```bash
docker compose exec api alembic upgrade head
```

## Web-кабинет

```bash
make web-install
make web-dev
make web-build
make web-deploy
```

## Landing

```bash
cd landing
npm install
npm run dev
npm run build
```

## Проверки качества

```bash
make format
make lint
make test
```

## Симуляция диалога

```bash
make simulate-dialog PHONE=+79990000000 SCENARIO=courier_screening
```
