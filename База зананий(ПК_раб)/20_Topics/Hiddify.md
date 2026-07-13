# Hiddify

Hiddify - клиентское приложение для импорта VLESS/subscription-профилей и подключения пользователя к VPN.

## Где используется

- [[Antygraviti MVP - Gatekeeper VPN Orchestrator]]
- `docs/client-setup.md`
- `docs/client-setup-android.md`
- `docs/client-setup-short.md`

## Роль

- импортировать VLESS-ссылку или subscription URL;
- включить system TUN;
- включить TLS fragmentation;
- использовать Reality-профиль или CDN-профиль;
- перехватывать трафик приложений на телефоне.

## Важные настройки из инструкции

- TLS tricks -> fragmentation включить;
- IPv6 route выключить;
- TUN implementation выбрать `system`;
- proxy внутри Telegram выключить;
- при timeout попробовать подключиться несколько раз или сменить сеть.

## Связи

- [[VLESS Reality]]
- [[Cloudflare CDN]]
- [[sing-box]]
