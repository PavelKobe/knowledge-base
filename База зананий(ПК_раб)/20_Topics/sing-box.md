# sing-box

sing-box - VPN-core, который запускается в Docker на VPS-ноде и принимает VLESS Reality/CDN-подключения.

## Где используется

- [[Antygraviti MVP - Gatekeeper VPN Orchestrator]]

## Файлы проекта

- `backend/src/orchestrator/ssh-deployer.ts`
- `backend/src/handlers/keys.ts`

## В Antygraviti MVP

- Docker image зафиксирован: `ghcr.io/sagernet/sing-box:v1.12.12`.
- Reality inbound слушает порт `443`.
- CDN inbound может слушать origin-порт, например `8443`.
- Список UUID пересобирается при добавлении/удалении ключа.
- Конфиг обновляется через SSH, затем контейнер перезапускается.

## Почему версия зафиксирована

В `ssh-deployer.ts` прямо указано, что `:latest` опасен: схема конфигурации sing-box между версиями меняется и может сломать новые ноды.

## Связи

- [[VLESS Reality]]
- [[Cloudflare CDN]]
- [[Aeza VPS]]
