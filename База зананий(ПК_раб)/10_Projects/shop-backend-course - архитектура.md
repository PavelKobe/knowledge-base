# shop-backend-course - архитектура

Связанный проект: [[shop-backend-course - учебный backend-стек]].

## Слои приложения

```mermaid
flowchart TD
    HTTP[HTTP request] --> Router[app/api/v1/products.py]
    Router --> Dependency[app/api/deps.py]
    Dependency --> Service[app/services/product.py]
    Service --> Repository[app/repositories/product.py]
    Repository --> Session[app/core/db.py AsyncSession]
    Session --> DB[(PostgreSQL)]
    Models[app/models] --> DB
    Schemas[app/schemas/product.py] --> Router
    Config[app/core/config.py] --> Session
```

## Роутеры

`app/api/v1/products.py` содержит CRUD для товаров:

- `GET /api/v1/products`
- `POST /api/v1/products`
- `GET /api/v1/products/{product_id}`
- `PATCH /api/v1/products/{product_id}`
- `DELETE /api/v1/products/{product_id}`

Роутер зависит от `ProductService`, который собирается через `Depends` в `app/api/deps.py`.

## Service layer

`ProductService` отвечает за бизнес-правила:

- получить список товаров;
- получить товар по id;
- создать slug из имени;
- проверить уникальность slug;
- обновить поля;
- удалить товар;
- превратить технические ситуации в доменные исключения `ProductNotFound`, `SlugAlreadyExists`.

Связь: [[Repository Service Pattern]].

## Repository layer

`ProductRepository` инкапсулирует SQLAlchemy-запросы:

- `select(Product).offset(skip).limit(limit).order_by(Product.id)`;
- `session.get(Product, product_id)`;
- поиск по `slug`;
- `add`, `commit`, `refresh`;
- `delete`.

## Модели

- `Product`: товар, цена, slug, описание, остаток, категория.
- `Category`: категория, slug, связь `products`.
- `User`: email, hashed_password, active/superuser flags.

Связи:

- [[SQLAlchemy 2 async]]
- [[PostgreSQL]]
- [[Alembic]]

## Конфигурация

`app/core/config.py` читает настройки через `pydantic-settings`:

```text
DATABASE_URL=postgresql+asyncpg://shop:shop@localhost:5432/shop
SECRET_KEY=change-me-in-env
ACCESS_TOKEN_EXPIRE_MINUTES=60
```

## Health check

`GET /health/db` выполняет `SELECT 1` через `AsyncSession`.

Если он падает, смотри [[Диагностика PostgreSQL и health-db]].
