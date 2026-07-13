# Yandex Cloud Serverless

Yandex Cloud Serverless в этом vault - набор managed/serverless-компонентов YC для приложения без постоянно работающего backend-сервера.

## Где используется

- [[Antygraviti MVP - Gatekeeper VPN Orchestrator]]

## Роль в Antygraviti MVP

- Cloud Function запускает TypeScript backend из `backend/src/index.ts`.
- API Gateway проксирует `/api/{proxy+}` в Cloud Function.
- Object Storage отдает React SPA и assets.
- Lockbox хранит продовые секреты.
- YDB Serverless хранит бизнес-данные.

## Файлы проекта

- `gateway-config.yaml`
- `deploy.ps1`
- `backend/src/index.ts`
- `backend/src/config/env.ts`

## Что помнить

Serverless снимает обслуживание постоянно запущенного приложения, но делает критичными:

- корректный deploy функции;
- переменные окружения и Lockbox;
- IAM/service account для API Gateway;
- cold start и длительные операции вроде provisioning из webhook.

## Связи

- [[Yandex Database YDB]]
- [[Yandex Object Storage]]
- [[JWT]]
