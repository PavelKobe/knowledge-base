# Voximplant

Voximplant - телефония для исходящих звонков, VoxEngine-сценариев, WebSocket streaming и SMS.

## Где используется

- [[VoiceScreen - голосовой AI-скрининг]]

## Файлы проекта

- `app/telephony/voximplant.py`
- `app/telephony/voxengine/screening.js`
- `app/api/webhooks.py`
- `app/api/ws.py`

## Роль в VoiceScreen

- старт исходящего звонка через Management API `StartScenarios`;
- передача `custom_data` в VoxEngine;
- открытие WebSocket на backend;
- события звонка через webhooks;
- получение recording URL;
- SMS через `SendSmsMessage`.
