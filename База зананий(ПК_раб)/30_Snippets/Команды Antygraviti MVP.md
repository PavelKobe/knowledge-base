# Команды Antygraviti MVP

Связанный проект: [[Antygraviti MVP - Gatekeeper VPN Orchestrator]].

Локальный путь:

```powershell
cd C:\Users\kobel\Project_Antygraviti\mvp_new
```

## Backend

```powershell
cd C:\Users\kobel\Project_Antygraviti\mvp_new\backend
npm install
npm run build
npm run dev:local
```

Утилиты backend:

```powershell
npm run list-aeza
npm run adopt-server
npm run adopt-shared
npm run apply-cdn
npm run apply-routing
npm run check:dpi
npm run check:generated-config
npm run check:subscription-links
npm run check:vpn-node-options
```

Прямые ts-node проверки, которые есть в `backend/src`:

```powershell
npx ts-node src/check-node-health.ts
npx ts-node src/check-node-config.ts
npx ts-node src/check-keys.ts
npx ts-node src/check-cdn-logs.ts
npx ts-node src/check-routing.ts
npx ts-node src/check-sub.ts
npx ts-node src/test-headers.ts
```

## Frontend

```powershell
cd C:\Users\kobel\Project_Antygraviti\mvp_new\frontend
npm install
npm run dev
npm run build
npm run lint
npm run preview
```

Обычно Vite dev-server:

```text
http://localhost:5173
```

## Полный деплой

Из корня проекта:

```powershell
cd C:\Users\kobel\Project_Antygraviti\mvp_new
.\deploy.ps1
```

`deploy.ps1` собирает backend/frontend, обновляет Cloud Function, загружает статику в Object Storage и обновляет API Gateway.

## Где смотреть конфигурацию

- `backend/.env` - локальные секреты, не коммитить.
- `backend/src/config/env.ts` - обязательный `JWT_SECRET`.
- `backend/src/config/tariffs.ts` - тарифы и лимиты.
- `backend/src/config/vpn-node.ts` - опции ноды, routing/CDN из env.
- `gateway-config.yaml` - маршруты API Gateway и статика.

## Связи

- [[Antygraviti MVP - архитектура]]
- [[Yandex Cloud Serverless]]
- [[Yandex Database YDB]]
- [[Aeza VPS]]
- [[sing-box]]
