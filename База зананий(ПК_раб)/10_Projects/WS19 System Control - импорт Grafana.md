# WS19 System Control - импорт Grafana

Готовый dashboard:

`Windows Server 2019 - WS19 System Control.dashboard.json`

Связанная инструкция: [[Windows Server 2019 - мониторинг Prometheus Grafana]]

## Что показывает dashboard

- Server UP / DOWN;
- uptime Windows Server;
- CPU %;
- RAM %;
- количество процессов;
- количество потоков;
- загрузку логических дисков;
- Disk Read / Write throughput;
- Top-10 процессов по CPU;
- Top-10 процессов по RAM;
- Top-10 процессов по I/O;
- сетевой RX / TX по интерфейсам;
- processor queue и page faults;
- автоматически запускаемые Windows Services, которые сейчас остановлены.

Dashboard рассчитан на job Prometheus:

```yaml
job_name: windows-server-2019
```

и на `windows_exporter` с collectors:

```text
cpu,cpu_info,logical_disk,memory,net,os,physical_disk,process,service,system,tcp
```

## Импорт

1. Убедиться, что `windows_exporter` работает.
2. В Prometheus проверить `Status -> Targets` и убедиться, что `windows-server-2019 = UP`.
3. В Grafana открыть `Dashboards`.
4. Нажать `New -> Import`.
5. Выбрать файл `Windows Server 2019 - WS19 System Control.dashboard.json`.
6. В поле datasource выбрать уже созданный Prometheus.
7. Нажать `Import`.

После импорта должен открыться dashboard:

```text
WS19 System Control
```

## Быстрая проверка до импорта

В Prometheus выполнить:

```promql
up{job="windows-server-2019"}
```

Должно быть:

```text
1
```

Далее проверить основные метрики:

```promql
windows_system_processes{job="windows-server-2019"}
```

```promql
windows_memory_physical_total_bytes{job="windows-server-2019"}
```

```promql
windows_logical_disk_size_bytes{job="windows-server-2019"}
```

```promql
windows_net_bytes_received_total{job="windows-server-2019"}
```

Для процессов обязательно должна возвращаться метрика:

```promql
windows_process_cpu_time_total{job="windows-server-2019"}
```

Если она отсутствует, collector `process` не включён.

## Особенности Top CPU

`windows_process_cpu_time_total` считает CPU-время отдельно для процессов и может суммироваться выше 100% на многопроцессорной системе. В готовом dashboard показатель нормализуется на количество логических CPU, поэтому Top-10 отображается как доля общей вычислительной мощности сервера.

## Диски

Dashboard показывает только тома с буквами:

```text
C:
D:
E:
...
```

Метрики `windows_logical_disk_free_bytes` и `windows_logical_disk_size_bytes` в Windows Performance Counters могут обновляться с задержкой примерно 10–15 минут. Это нормальное поведение источника метрик.

## Windows Services

Нижняя таблица показывает службы с:

```text
start_mode = auto
state = stopped
```

Это удобнее, чем показывать все остановленные службы Windows: многие manual/disabled services штатно не работают постоянно.

Для отдельной критичной службы можно создать дополнительную Stat-панель:

```promql
windows_service_state{name="ИМЯ_СЛУЖБЫ",state="running"} == 1
```

Узнать системное имя службы:

```powershell
Get-Service | Select-Object Name,DisplayName,Status | Sort-Object Name
```

## Если панели процессов пустые

Проверить:

```powershell
Invoke-WebRequest http://127.0.0.1:9182/metrics -UseBasicParsing | Select-String "windows_process_cpu_time_total"
```

Если метрики нет — проверить параметры службы:

```powershell
Get-CimInstance Win32_Service -Filter "Name='windows_exporter'" | Select-Object Name,State,PathName
```

Collector `process` должен быть включён явно.

## Следующая адаптация под корпоративный WS19

После первого импорта рекомендуется добавить отдельную строку `Corporate Applications` и вывести туда только действительно критичные процессы и службы, например:

- backend/API;
- PostgreSQL;
- Flask/FastAPI-приложения;
- Node.js;
- nginx/reverse proxy;
- внутренние Windows Services.

Для этого сначала получить реальные имена процессов и служб на сервере:

```powershell
Get-Process | Sort-Object ProcessName | Select-Object ProcessName,Id
```

```powershell
Get-Service | Where-Object Status -eq 'Running' | Select-Object Name,DisplayName
```

После этого dashboard можно сделать не только системным монитором, но и экраном контроля конкретных корпоративных приложений.