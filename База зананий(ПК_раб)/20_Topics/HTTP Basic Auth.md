# HTTP Basic Auth

HTTP Basic Auth - простой механизм авторизации через заголовок `Authorization`.

## Где используется

В проекте [[retail-audit-kit - корпоративный аудит розницы]] реализован в `backend/auth.py`.

## Как устроено

Пользователи читаются из `.env`:

- `USERS_admin` - JSON-массив администраторов;
- `USERS_users` - JSON-массив обычных пользователей;
- fallback: `ADMIN_USERNAME` / `ADMIN_PASSWORD`.

Роли:

- `admin` - создание проверок, история, статистика, Excel, конструктор шаблонов, журнал действий, KPI;
- `user` - просмотр истории своего объекта и обратная связь директора.

## Важная деталь

Пароль сравнивается через `secrets.compare_digest`, чтобы избежать простого timing leak.

## Связи

- [[FastAPI]]
- [[retail-audit-kit - данные и API]]
