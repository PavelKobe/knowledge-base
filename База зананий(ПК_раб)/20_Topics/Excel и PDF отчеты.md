# Excel и PDF отчеты

В корпоративных приложениях выгрузки часто не дополнение, а основной способ передачи результата бизнесу.

## Где используется

В проекте [[retail-audit-kit - корпоративный аудит розницы]].

## Backend-файлы

- `backend/export_excel.py`
- `backend/export_pdf.py` упоминается в документации, но в текущем дереве основной видимый файл - Excel export

## Зависимости

- `openpyxl`
- `reportlab`
- frontend: `xlsx`

## Endpoints

- `GET /api/sb_retail_checks/{id}/excel`
- `POST /api/sb_retail_checks/stats/excel`
- `POST /api/sb_retail_checks/violations/excel`
- `POST /api/kpi/performance/excel`

## Связи

- [[KPI отчеты]]
- [[Checklist Templates]]
- [[FastAPI]]
