# shop-backend-course - карта модулей

Связанный проект: [[shop-backend-course - учебный backend-стек]].

## Логика курса

Курс собирает backend интернет-магазина по слоям: от минимального [[FastAPI]] приложения до production-сборки с [[Docker Compose]], [[Nginx Prometheus]] и фоновыми задачами.

## Модули

| Модуль | Тема | Узлы Obsidian |
|---|---|---|
| M00 | Окружение и структура проекта | [[Docker Compose]], [[FastAPI]] |
| M01 | FastAPI маршруты и автодокументация | [[FastAPI]] |
| M02 | Pydantic v2 и настройки | [[Pydantic v2]] |
| M03 | PostgreSQL + SQLAlchemy async | [[PostgreSQL]], [[SQLAlchemy 2 async]] |
| M04 | Alembic миграции | [[Alembic]] |
| M05 | Доменные модели: товары и категории | [[SQLAlchemy 2 async]], [[PostgreSQL]] |
| M06 | CRUD + service/repository | [[Repository Service Pattern]] |
| M07 | Пользователи и авторизация | [[JWT]] |
| M08 | Корзина и заказы | [[PostgreSQL]], [[Repository Service Pattern]] |
| M09 | Jinja2-витрина | [[Jinja2]] |
| M10 | SQLAdmin | [[SQLAdmin]] |
| M11 | Redis кеширование | [[Redis]] |
| M12 | Celery + RabbitMQ | [[Celery]], [[RabbitMQ]] |
| M13 | Тестирование | [[Pytest]] |
| M14 | Качество кода | [[Ruff Mypy pre-commit]] |
| M15 | Финальная сборка | [[Nginx Prometheus]], [[Docker Compose]] |

## Как учить

1. Сначала понять поток запроса: [[shop-backend-course - архитектура]].
2. Потом пройти базу: [[FastAPI]], [[Pydantic v2]], [[PostgreSQL]], [[SQLAlchemy 2 async]], [[Alembic]].
3. Затем закрепить архитектуру слоев: [[Repository Service Pattern]].
4. После этого добавлять прикладные темы: [[JWT]], [[Redis]], [[Celery]], [[RabbitMQ]].
