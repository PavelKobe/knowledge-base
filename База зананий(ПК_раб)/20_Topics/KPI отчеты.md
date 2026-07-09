# KPI отчеты

KPI-отчеты в проекте [[retail-audit-kit - корпоративный аудит розницы]] связаны с производительностью проверок и Excel-выгрузкой.

## Где смотреть

- `backend/kpi_performance.py`
- `backend/export_excel.py`
- `frontend/src/components/KpiSection.jsx`
- `frontend/src/components/KpiPerformanceReport.jsx`
- `frontend/src/utils/kpi.js`

## API

- `GET /api/kpi/performance?month=YYYY-MM`
- `POST /api/kpi/performance/excel`

## Доступ

KPI endpoints доступны только роли `admin`.

## Связи

- [[Excel и PDF отчеты]]
- [[HTTP Basic Auth]]
- [[PostgreSQL]]
