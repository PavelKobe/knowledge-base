# Celery

Celery выполняет фоновые задачи вне HTTP-запроса.

## В курсе

Модуль M12: Celery + [[RabbitMQ]] для фоновых задач, например писем о заказах.

## Поток

```mermaid
flowchart LR
    FastAPI[[FastAPI]] --> Broker[[RabbitMQ]]
    Broker --> Worker[Celery worker]
    Worker --> Result[Side effect: email / report / external API]
```

## Связи

- [[RabbitMQ]]
- [[Redis]]
- [[Docker Compose]]
- [[FastAPI]]
