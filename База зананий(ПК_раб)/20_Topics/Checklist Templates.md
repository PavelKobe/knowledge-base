# Checklist Templates

Checklist Templates - ключевая предметная модель проекта [[retail-audit-kit - корпоративный аудит розницы]].

## Зачем нужны

Шаблон описывает форму проверки: временные слоты, блоки, критерии и варианты ответов.

## Где смотреть

- `backend/checklist_templates.py`
- `backend/sb_retail_seed.py`
- `frontend/src/components/ChecklistConstructor.jsx`
- `frontend/src/components/ChecklistTemplateEditor.jsx`
- `frontend/src/components/TemplatePicker.jsx`
- `frontend/src/utils/checklist_definition.js`

## Основные поля

- `slug` - служебный код;
- `name` - имя для пользователя;
- `definition` - JSONB-описание формы;
- `is_active` - доступность шаблона.

## Snapshot

При сохранении проверки backend кладет текущий шаблон в `payload.template_snapshot`.

Это важно: если администратор изменит шаблон, старые проверки останутся воспроизводимыми по старой структуре.

## Связи

- [[React]]
- [[FastAPI]]
- [[PostgreSQL]]
- [[retail-audit-kit - данные и API]]
