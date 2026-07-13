# JWT

JWT нужен для авторизации пользователей: регистрация, вход, access token, роли.

## В проекте

В `requirements.txt` есть `PyJWT`. В модели `User` уже есть поля:

- `email`
- `hashed_password`
- `is_active`
- `is_superuser`

## Связи

- [[FastAPI]]
- [[Pydantic v2]]
- [[PostgreSQL]]
