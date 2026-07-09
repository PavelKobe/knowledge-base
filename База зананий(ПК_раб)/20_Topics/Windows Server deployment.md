# Windows Server deployment

Windows Server deployment - запуск приложения во внутренней корпоративной сети.

## Где используется

- [[retail-audit-kit - корпоративный аудит розницы]]

## Особенности

- PostgreSQL on-premises;
- backend через Uvicorn workers;
- frontend через Vite preview или статическую раздачу;
- настройка firewall;
- переменные окружения в `.env`;
- доступ к UNC/share для фото.

## Связи

- [[FastAPI]]
- [[React]]
- [[PostgreSQL]]
- [[Корпоративное хранилище фото]]
- [[Деплой retail-audit-kit на Windows Server]]
