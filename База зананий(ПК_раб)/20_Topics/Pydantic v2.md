# Pydantic v2

Pydantic отвечает за валидацию входных и выходных данных.

## Где смотреть

- `app/schemas/product.py`
- `app/core/config.py`

## В проекте

`ProductCreate`, `ProductUpdate`, `ProductRead` описывают контракт API для товаров.

`Settings` читает настройки из `.env` через `pydantic-settings`.

## Ключевые идеи

- `Field(...)` задает ограничения.
- `ConfigDict(from_attributes=True)` позволяет строить ответ из SQLAlchemy-модели.
- `BaseSettings` полезен для конфигурации приложения.

## Связи

- [[FastAPI]]
- [[SQLAlchemy 2 async]]
