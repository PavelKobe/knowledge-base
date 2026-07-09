# SQLAlchemy 2 async

SQLAlchemy используется как ORM и слой доступа к базе.

## Где смотреть

- `app/core/db.py`
- `app/models/base.py`
- `app/models/product.py`
- `app/models/category.py`
- `app/models/user.py`
- `app/repositories/product.py`

## В проекте

`create_async_engine` создает async engine.

`async_sessionmaker` создает `SessionLocal`.

`get_session` отдает `AsyncSession` в FastAPI dependency.

## Модели

- `Product`
- `Category`
- `User`

## Связи

- [[PostgreSQL]]
- [[Alembic]]
- [[Repository Service Pattern]]
- [[FastAPI]]
