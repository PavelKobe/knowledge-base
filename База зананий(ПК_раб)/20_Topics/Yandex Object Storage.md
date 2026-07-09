# Yandex Object Storage

Yandex Object Storage - S3-совместимое хранилище для записей звонков.

## Где используется

- [[VoiceScreen - голосовой AI-скрининг]]

## Файлы проекта

- `app/storage/yos.py`
- `app/workers/tasks.py`

## Роль

- скачать запись из Voximplant;
- загрузить MP3 в Object Storage;
- сохранить URI в `Call.recording_url`;
- выдать HR временную presigned-ссылку.

Важно: нельзя отдавать клиенту внутренний Voximplant URL с API key.
