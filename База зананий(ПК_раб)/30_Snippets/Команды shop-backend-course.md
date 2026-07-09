# Команды shop-backend-course

Связанный проект: [[shop-backend-course - учебный backend-стек]].

## Перейти в проект

```powershell
cd "C:\Users\p.kobelev\Курс_интернет магазин\shop-backend-course"
```

## Виртуальное окружение

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

## Запуск API

```powershell
uvicorn app.main:app --reload
uvicorn app.main:app --reload --port 8001
```

Swagger:

```text
http://127.0.0.1:8000/docs
http://127.0.0.1:8001/docs
```

## PostgreSQL через Docker

```powershell
docker compose up -d
docker compose ps
docker compose logs -f db
```

## Health check

```text
http://127.0.0.1:8000/health/db
```

Ожидаемый ответ:

```json
{"db": 1}
```

## Alembic

```powershell
alembic revision --autogenerate -m "create tables"
alembic upgrade head
```

## Seed данные

```powershell
python -m scripts.seed
```

## Связи

- [[FastAPI]]
- [[PostgreSQL]]
- [[Alembic]]
- [[Диагностика PostgreSQL и health-db]]
