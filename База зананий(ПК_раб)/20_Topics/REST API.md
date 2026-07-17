# REST API

REST API - это стиль проектирования HTTP API вокруг ресурсов. Клиент обращается к понятным сущностям через стандартные HTTP-методы, а сервер возвращает представление ресурса, статус операции, заголовки и ошибки в предсказуемом формате.

В проектах базы REST API обычно реализуется через [[FastAPI]], схемы контрактов через [[Pydantic v2]], доступ к данным через [[Repository Service Pattern]], [[SQLAlchemy 2 async]] и [[PostgreSQL]].

## Ключевые принципы и концепции

### 1. Клиент-сервер

Клиент и сервер разделены по ответственности.

Клиент отвечает за:

- интерфейс;
- хранение локального UI-состояния;
- отправку запросов;
- обработку статусов и ошибок.

Сервер отвечает за:

- бизнес-правила;
- данные;
- авторизацию;
- валидацию;
- единый контракт API.

Пример:

```text
React/Vite frontend  ->  FastAPI backend  ->  PostgreSQL
```

Frontend не должен знать SQL-структуру таблиц, а backend не должен зависеть от того, как именно экран рисует карточку товара.

### 2. Ресурсность

REST API описывает не действия в стиле `doSomething`, а ресурсы.

Плохо:

```text
POST /createProduct
POST /deleteProduct
POST /getUserOrders
```

Лучше:

```text
POST   /products
DELETE /products/{product_id}
GET    /users/{user_id}/orders
```

Ресурс - это предметная сущность: `products`, `orders`, `users`, `checks`, `templates`, `candidates`, `calls`.

URL отвечает на вопрос "с чем работаем", а HTTP-метод - "что делаем".

```text
GET    /products              # список товаров
POST   /products              # создать товар
GET    /products/{id}         # получить товар
PATCH  /products/{id}         # частично изменить товар
DELETE /products/{id}         # удалить товар
```

### 3. Представление ресурса

Клиент не получает сам объект из базы данных. Он получает представление ресурса: чаще всего JSON.

Один и тот же ресурс может иметь разные представления:

```text
GET /products/10              # полная карточка товара
GET /products/10?view=short   # короткое представление для списка
```

Пример ответа:

```json
{
  "id": 10,
  "name": "Keyboard",
  "price": 4500,
  "is_active": true
}
```

Важно отделять API-схемы от ORM-моделей. Внутри БД могут быть служебные поля, которые нельзя отдавать наружу.

Связи: [[Pydantic v2]], [[SQLAlchemy 2 async]].

### 4. Stateless

Каждый запрос содержит все, что нужно серверу для обработки. Сервер не должен догадываться о контексте из предыдущего запроса.

Практически это значит:

- авторизация передается в каждом запросе: cookie, session token, JWT или basic auth;
- фильтры, пагинация и сортировка передаются явно;
- тело запроса содержит все данные для операции;
- результат не зависит от скрытого состояния разговора клиента с сервером.

Пример stateless-запроса:

```http
GET /api/v1/products?limit=20&offset=0 HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
```

Плохо, если сервер ожидает: "пользователь на прошлом шаге выбрал категорию, значит здесь я сам ее вспомню". Лучше передать категорию явно:

```text
GET /products?category_id=5&limit=20
```

Связи: [[JWT]], [[Session Cookie Auth]], [[HTTP Basic Auth]].

### 5. Кэшируемость

Ответ REST API должен явно показывать, можно ли его кэшировать. Это снижает нагрузку на backend и ускоряет интерфейс.

Что обычно можно кэшировать:

- справочники;
- публичные карточки товаров;
- настройки интерфейса;
- редко меняющиеся списки;
- статичные файлы и изображения.

Что обычно нельзя кэшировать без осторожности:

- персональные данные;
- корзину;
- платежи;
- админские списки;
- данные, которые часто меняются.

Пример кэшируемого ответа:

```http
HTTP/1.1 200 OK
Cache-Control: public, max-age=3600
ETag: "product-10-v3"
Content-Type: application/json
```

Повторный запрос с проверкой версии:

```http
GET /products/10 HTTP/1.1
If-None-Match: "product-10-v3"
```

Если ресурс не изменился:

```http
HTTP/1.1 304 Not Modified
```

Пример запрета кэширования приватного ответа:

```http
HTTP/1.1 200 OK
Cache-Control: no-store
Content-Type: application/json
```

Минимальное правило: для приватных endpoint по умолчанию `Cache-Control: no-store`, для публичных справочников можно `public, max-age=...`.

### 6. Единый интерфейс

REST держится на единых правилах:

- стандартные HTTP-методы;
- стандартные HTTP-статусы;
- единые URL-шаблоны;
- единый формат ошибок;
- единые правила пагинации, фильтрации и сортировки;
- единые заголовки для auth, cache, content type.

Если один endpoint возвращает ошибку строкой, второй JSON-объектом, а третий всегда `200 OK`, API перестает быть предсказуемым.

### 7. Идемпотентность

Идемпотентная операция дает один и тот же итог при повторном выполнении.

| Метод | Обычно идемпотентен | Комментарий |
|---|---:|---|
| `GET` | да | не должен менять данные |
| `PUT` | да | повторная замена теми же данными не меняет итог |
| `PATCH` | зависит | безопаснее делать предсказуемым |
| `DELETE` | да | повторное удаление не должно ломать систему |
| `POST` | нет | повторный запрос может создать дубль |

Пример проблемы:

```text
POST /orders
```

Если клиент отправит запрос два раза из-за сетевого сбоя, может появиться два заказа. Для таких операций нужен `Idempotency-Key`.

```http
POST /orders HTTP/1.1
Idempotency-Key: 7d7f6b7e-5d3f-4d1f-8f5c-001
```

### 8. Слоистая система

Клиент не обязан знать, отвечает ему сам backend, gateway, reverse proxy, CDN или балансировщик. Это позволяет добавлять слои без изменения клиента.

Типовая схема:

```text
Browser -> CDN/Nginx -> FastAPI -> PostgreSQL
```

Практический вывод:

- не зашивать внутренние адреса сервисов в клиент;
- корректно работать с заголовками;
- проектировать API так, чтобы между клиентом и сервером мог появиться proxy/cache.

Связь: [[Nginx Prometheus]], [[Cloudflare CDN]].

### 9. Code on demand

В классическом REST есть опциональный принцип code on demand: сервер может отдавать клиенту исполняемый код. В обычных backend API это почти не используется.

Для наших проектов достаточно помнить: REST API обычно отдает данные, а не динамический код поведения.

### 10. HATEOAS

HATEOAS - идея, что ответ API может содержать ссылки на доступные следующие действия.

Пример:

```json
{
  "id": 15,
  "status": "draft",
  "links": {
    "self": "/orders/15",
    "pay": "/orders/15/pay",
    "cancel": "/orders/15/cancel"
  }
}
```

В строгом REST это важная концепция. В практических FastAPI-проектах ее часто используют частично: например, добавляют ссылки на `self`, `next`, `prev` в пагинации или на связанные ресурсы.

## HTTP-методы

| Метод | Смысл | Пример |
|---|---|---|
| `GET` | получить данные без изменения состояния | `GET /products` |
| `POST` | создать ресурс или запустить операцию | `POST /orders` |
| `PUT` | полностью заменить ресурс | `PUT /products/{id}` |
| `PATCH` | частично изменить ресурс | `PATCH /products/{id}` |
| `DELETE` | удалить ресурс | `DELETE /products/{id}` |

Важная разница:

- `PUT` обычно требует полное состояние объекта;
- `PATCH` передает только изменяемые поля;
- `POST` подходит для создания и для командных операций, которые не ложатся на CRUD.

Командные операции допустимы, если они отражают предметную область:

```text
POST /orders/{id}/pay
POST /calls/{id}/retry
POST /reports/{id}/export
```

## HTTP-статусы

| Статус | Когда использовать |
|---|---|
| `200 OK` | успешный ответ с телом |
| `201 Created` | ресурс создан |
| `204 No Content` | успешно, тела ответа нет |
| `304 Not Modified` | кэшированная версия актуальна |
| `400 Bad Request` | запрос синтаксически или логически неверен |
| `401 Unauthorized` | пользователь не аутентифицирован |
| `403 Forbidden` | пользователь известен, но прав нет |
| `404 Not Found` | ресурс не найден |
| `409 Conflict` | конфликт состояния, дубль, гонка, уже завершено |
| `422 Unprocessable Entity` | ошибка валидации данных |
| `429 Too Many Requests` | превышен лимит запросов |
| `500 Internal Server Error` | неожиданная ошибка сервера |
| `503 Service Unavailable` | зависимость недоступна или сервис временно не готов |

## Ошибки

Ошибки должны быть одинаковыми во всем API. Не смешивать строки, разные JSON-форматы и HTML-страницы.

Хороший формат:

```json
{
  "error": {
    "code": "product_not_found",
    "message": "Product not found",
    "details": {
      "product_id": 123
    }
  }
}
```

Минимальные правила:

- `code` стабилен и удобен для frontend;
- `message` можно показать человеку или залогировать;
- `details` содержит безопасные технические детали;
- не возвращать stack trace, SQL, секреты и персональные данные.

## Query-параметры

Query-параметры подходят для чтения, фильтрации, сортировки и пагинации.

```text
GET /products?limit=20&offset=0&category_id=5&sort=-created_at
GET /checks?date_from=2026-07-01&date_to=2026-07-31&status=done
```

Типовой набор:

- `limit`, `offset` или cursor-pagination;
- `sort`, например `created_at` или `-created_at`;
- фильтры по статусу, дате, владельцу, категории;
- полнотекстовый поиск через `q`.

## Пагинация

Для простых админок достаточно:

```text
GET /products?limit=50&offset=100
```

Ответ:

```json
{
  "items": [],
  "total": 256,
  "limit": 50,
  "offset": 100
}
```

Для больших таблиц и лент лучше cursor-pagination:

```text
GET /events?limit=50&cursor=eyJpZCI6...
```

Ответ с HATEOAS-элементами:

```json
{
  "items": [],
  "next": "/events?limit=50&cursor=next_cursor",
  "prev": null
}
```

## Версионирование

Для backend-проектов базы удобный стандарт:

```text
/api/v1/products
/api/v1/orders
/api/v1/users
```

Версию повышать, когда ломается контракт:

- меняется структура ответа;
- удаляется поле;
- меняется смысл поля;
- меняются коды ошибок;
- меняются правила авторизации.

Не нужно повышать версию, если добавлено необязательное поле или новый endpoint.

## Безопасность

- [ ] Все endpoint с приватными данными требуют авторизацию.
- [ ] Проверяются права на конкретный ресурс, а не только факт логина.
- [ ] Секреты и токены не попадают в ответы и логи.
- [ ] Ошибки не раскрывают внутренности БД и stack trace.
- [ ] Для публичных endpoint есть rate limit.
- [ ] Для CORS разрешены только нужные origin, а не `*` в production.
- [ ] Для файлов проверяются размер, тип и путь хранения.
- [ ] Для приватных ответов задано `Cache-Control: no-store`.

Связи: [[JWT]], [[HTTP Basic Auth]], [[Session Cookie Auth]].

## Контракт в FastAPI

Минимальный пример:

```python
from fastapi import APIRouter, HTTPException, Response, status
from pydantic import BaseModel, Field

router = APIRouter(prefix="/products", tags=["products"])

class ProductCreate(BaseModel):
    name: str = Field(min_length=1, max_length=200)
    price: int = Field(ge=0)

class ProductRead(BaseModel):
    id: int
    name: str
    price: int

@router.post("", response_model=ProductRead, status_code=status.HTTP_201_CREATED)
async def create_product(payload: ProductCreate) -> ProductRead:
    product = await service.create_product(payload)
    return ProductRead.model_validate(product)

@router.get("/{product_id}", response_model=ProductRead)
async def get_product(product_id: int, response: Response) -> ProductRead:
    product = await service.get_product(product_id)
    if product is None:
        raise HTTPException(status_code=404, detail="Product not found")
    response.headers["Cache-Control"] = "public, max-age=300"
    return ProductRead.model_validate(product)
```

## Чек-лист проектирования endpoint

- [ ] URL называется ресурсом во множественном числе: `/products`, `/orders`, `/users`.
- [ ] HTTP-метод соответствует смыслу операции.
- [ ] Есть Pydantic-схема входа.
- [ ] Есть Pydantic-схема ответа.
- [ ] Описаны статусы успеха и ошибок.
- [ ] Ошибка возвращается в едином формате.
- [ ] Для списков есть пагинация.
- [ ] Для публичных GET-ответов продумана кэшируемость.
- [ ] Для приватных данных есть auth, проверка прав и запрет кэширования.
- [ ] Контракт покрыт тестами.
- [ ] Swagger/OpenAPI показывает понятные имена и примеры.

## Частые ошибки

| Ошибка | Почему плохо | Как лучше |
|---|---|---|
| `GET /deleteProduct?id=1` | GET меняет состояние | `DELETE /products/1` |
| `POST /getProducts` | действие вместо ресурса | `GET /products` |
| Везде `200 OK` | frontend не понимает тип ошибки | использовать `404`, `409`, `422`, `403` |
| Разные форматы ошибок | сложно обрабатывать на клиенте | единый error envelope |
| Нет пагинации | списки ломаются на больших данных | `limit/offset` или cursor |
| Нет cache headers | proxy и browser не понимают, можно ли хранить ответ | `Cache-Control`, `ETag` |
| Кэшируются приватные данные | риск утечки через browser/proxy cache | `Cache-Control: no-store` |
| Нет `response_model` | контракт расползается | Pydantic-схемы на вход и выход |
| Схемы БД = схемы API | наружу утекает внутренняя модель | отдельные API schemas |

## Что тестировать

- [ ] Успешное создание ресурса.
- [ ] Получение списка с пагинацией.
- [ ] Получение одного ресурса.
- [ ] Ошибка `404`, если ресурса нет.
- [ ] Ошибка `422`, если входные данные неверные.
- [ ] Ошибка `401/403`, если нет доступа.
- [ ] Конфликт `409`, если состояние не позволяет операцию.
- [ ] Формат ответа соответствует схеме.
- [ ] Для публичных GET есть ожидаемые `Cache-Control`/`ETag`.
- [ ] Для приватных endpoint стоит `Cache-Control: no-store`.

Связь: [[Pytest]].

## Связи

- [[FastAPI]]
- [[Pydantic v2]]
- [[Repository Service Pattern]]
- [[SQLAlchemy 2 async]]
- [[PostgreSQL]]
- [[JWT]]
- [[HTTP Basic Auth]]
- [[Session Cookie Auth]]
- [[Pytest]]
- [[Nginx Prometheus]]
- [[Cloudflare CDN]]
