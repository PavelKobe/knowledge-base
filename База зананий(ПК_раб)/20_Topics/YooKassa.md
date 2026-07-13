# YooKassa

YooKassa - платежная система. В Antygraviti MVP webhook оплаты запускает создание подписки и provisioning VPN-ноды.

## Где используется

- [[Antygraviti MVP - Gatekeeper VPN Orchestrator]]
- [[Antygraviti MVP - данные и API]]

## Файл проекта

- `backend/src/handlers/billing.ts`

## Основной поток

1. YooKassa отправляет `payment.succeeded` на `/api/billing/webhook`.
2. Бэкенд берет `metadata.userId` и `metadata.tariffId`.
3. Проверяет тариф.
4. Делает dedupe по `payment_id`.
5. Создает/переиспользует подписку.
6. Запускает dedicated или shared provisioning.

## Что помнить

- Metadata платежа критична: без `userId` и `tariffId` webhook не сможет привязать оплату.
- Перед production нужно проверить защиту webhook: подпись, источник, idempotency и логи.
- Webhook сейчас отвечает 200 даже на часть бизнес-отказов, чтобы не плодить ретраи платежной системы.

## Связи

- [[Yandex Cloud Serverless]]
- [[Aeza VPS]]
- [[Yandex Database YDB]]
