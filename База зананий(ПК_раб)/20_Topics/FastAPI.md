# FastAPI

FastAPI - web-фреймворк для создания API на Python.

В проекте [[shop-backend-course - учебный backend-стек]] FastAPI используется как основной слой HTTP API.

## Где смотреть в проекте

- `app/main.py` - создание `FastAPI(title="Shop API", version="0.1.0")`.
- `app/api/v1/products.py` - роуты товаров.
- `app/api/deps.py` - зависимости через `Depends`.

## Что важно понять

- `APIRouter` группирует endpoints.
- `Depends` внедряет зависимости.
- `response_model` связывает ответ API с [[Pydantic v2]].
- Swagger доступен по `/docs`.

## Связи

- [[Pydantic v2]]
- [[Repository Service Pattern]]
- [[SQLAlchemy 2 async]]
