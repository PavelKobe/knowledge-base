# Команды retail-audit-kit

Связанный проект: [[retail-audit-kit - корпоративный аудит розницы]].

## Клонирование

```powershell
git clone git@github.com:PavelKobe/retail-audit-kit.git
cd retail-audit-kit
```

## Backend dev

```powershell
cd backend
py -3.11 -m venv venv
.\venv\Scripts\Activate.ps1
pip install -r requirements.txt
copy .env.example .env
python -m uvicorn main:app --reload --host 127.0.0.1 --port 8000
```

Swagger:

```text
http://localhost:8000/docs
```

## Frontend dev

```powershell
cd frontend
npm install
copy .env.example .env
npm run dev
```

Открыть:

```text
http://localhost:5173
```

## База и миграции

```powershell
cd backend
python -m alembic upgrade head
python -m alembic current
```

## Production-like запуск

Из корня проекта:

```bat
start_app_prod.bat
```

Production:

```bat
start_prod.bat
```

Остановка:

```bat
stop_app_prod.bat
```

## Связи

- [[FastAPI]]
- [[React]]
- [[Vite]]
- [[PostgreSQL]]
- [[Alembic]]
- [[Деплой retail-audit-kit на Windows Server]]
