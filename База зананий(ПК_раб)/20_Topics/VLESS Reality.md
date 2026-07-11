# VLESS Reality

VLESS Reality - основной VPN-профиль в Antygraviti MVP. Клиент подключается к VPS на 443, а traffic маскируется под TLS/Reality handshake.

## Где используется

- [[Antygraviti MVP - Gatekeeper VPN Orchestrator]]
- [[Antygraviti MVP - архитектура]]

## Файлы проекта

- `backend/src/handlers/key-links.ts`
- `backend/src/orchestrator/ssh-deployer.ts`
- `docs/client-setup.md`

## Формат ссылки

Ссылка собирается в `buildConfigString`:

```text
vless://<uuid>@<ip>:443?encryption=none&security=reality&sni=www.microsoft.com&fp=chrome&pbk=<publicKey>&sid=<shortId>&flow=xtls-rprx-vision#<label>
```

## Практическая роль

- быстрый прямой профиль на IP ноды;
- работает вместе с Hiddify и TLS fragmentation;
- остается fallback, если включен CDN-профиль;
- использует publicKey/shortId, сгенерированные sing-box на ноде.

## Связи

- [[sing-box]]
- [[Hiddify]]
- [[Cloudflare CDN]]
