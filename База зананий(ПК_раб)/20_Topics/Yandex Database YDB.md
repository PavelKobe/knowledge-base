# Yandex Database YDB

YDB - serverless database в Yandex Cloud. В Antygraviti MVP используется как основное хранилище пользователей, подписок, нод и VPN-ключей.

## Где используется

- [[Antygraviti MVP - Gatekeeper VPN Orchestrator]]
- [[Antygraviti MVP - данные и API]]

## Файлы проекта

- `backend/src/db/ydb-client.ts`
- `backend/src/db/migrations.sql`
- `backend/src/db/backfill-2026-05.sql`

## Таблицы

- `users`
- `subscriptions`
- `servers`
- `vpn_keys`

## Конвенции

- Все запросы параметризуются через `DECLARE $x AS ...`.
- Значения конвертируются в typed values внутри `ydb-client.ts`.
- `YdbClient.getInstance()` работает как singleton для переиспользования между вызовами serverless-функции.
- UUID используются как primary key; автоинкремента нет.

## Особенность

`servers.config_data` - JSON-строка с чувствительными данными ноды: root-пароль, Reality private/public key, shortId, routing/CDN.

## Связи

- [[Yandex Cloud Serverless]]
- [[VLESS Reality]]
- [[sing-box]]
