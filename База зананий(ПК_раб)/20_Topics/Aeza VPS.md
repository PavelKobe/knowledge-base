# Aeza VPS

Aeza VPS - провайдер виртуальных серверов, на которых Antygraviti MVP поднимает VPN-ноды.

## Где используется

- [[Antygraviti MVP - Gatekeeper VPN Orchestrator]]
- [[Эксплуатация Antygraviti MVP]]

## Файлы проекта

- `backend/src/orchestrator/aeza-client.ts`
- `backend/src/orchestrator/orchestrator.ts`
- `backend/src/list-aeza.ts`

## Роль

- заказать VPS;
- дождаться active/IP;
- получить или сменить root-пароль;
- удалить осиротевший сервер при ошибке provisioning;
- хранить `external_id` сервера в таблице `servers`.

## Важное поведение

После заказа VPS деньги/баланс уже затронуты, поэтому все дальнейшие шаги provisioning обернуты в cleanup: при сбое код пытается удалить сервер через API.

## Связи

- [[sing-box]]
- [[VLESS Reality]]
- [[Yandex Database YDB]]
