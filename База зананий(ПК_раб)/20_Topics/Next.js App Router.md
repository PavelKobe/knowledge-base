# Next.js App Router

Next.js App Router - файловая маршрутизация Next.js через каталог `src/app`, server/client components и route handlers.

## Где используется

- [[NutryAI - ИИ-дневник питания]]

## Файлы проекта

- `app/web/src/app/layout.tsx`
- `app/web/src/app/page.tsx`
- `app/web/src/app/dashboard/page.tsx`
- `app/web/src/app/admin/*`
- `app/web/src/app/api/v1/aihub/[...path]/route.ts`
- `app/web/next.config.ts`

## Роль в NutryAI

- лендинг и пользовательские страницы;
- dashboard, analytics, add-food, chat, meal-plan, products;
- admin UI;
- PWA manifest/offline pages;
- dev rewrite `/api/*` на FastAPI backend;
- proxy для AIHub streaming route.

## Связи

- [[React]]
- [[PWA]]
- [[AIHub]]
