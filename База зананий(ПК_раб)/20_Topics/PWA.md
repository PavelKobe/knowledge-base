# PWA

PWA - Progressive Web App: веб-приложение, которое можно установить на телефон/desktop, с manifest, service worker и offline/notification capabilities.

## Где используется

- [[NutryAI - ИИ-дневник питания]]

## Файлы проекта

- `app/web/src/app/manifest.ts`
- `app/web/public/sw.js`
- `app/web/src/components/ServiceWorkerRegistrar.tsx`
- `app/web/src/components/pwa/PWAInstallBanner.tsx`
- `app/web/src/components/pwa/ManualInstallInstructionsModal.tsx`
- `app/web/src/app/offline/page.tsx`
- `app/backend/routers/push.py`
- `app/backend/models/push_subscription.py`

## Роль

- install prompt/banner;
- offline page;
- push subscriptions;
- VAPID public key endpoint;
- уведомления, в том числе coaching unread counts.

## Связи

- [[Next.js App Router]]
- [[NutryAI - архитектура]]
