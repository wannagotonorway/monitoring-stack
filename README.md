# Monitoring Stack

Полноценный стек мониторинга с Prometheus, Grafana и экспортерами для мониторинга хоста и Nginx.

## Что мониторим

- **Хост**: CPU, RAM, диск, сеть, процессы (через node-exporter)
- **Nginx**: HTTP запросы, статусы, response time (через nginx-exporter)
- **Prometheus**: собственные метрики
- **Grafana**: визуализация всех метрик

## Запуск

```bash
# Запустить все сервисы
docker-compose up -d

# Посмотреть логи
docker-compose logs -f

# Остановить
docker-compose down
```

## Доступ к сервисам

- **Prometheus**: http://localhost:9090
- **Grafana**: http://localhost:3000 (admin/admin)
- **Nginx**: http://localhost:8080
- **Nginx Status**: http://localhost:8080/nginx_status

## Структура проекта

```
monitoring-stack/
├── docker-compose.yml          # Основной файл с сервисами
├── prometheus/
│   └── prometheus.yml         # Конфигурация Prometheus
├── grafana/
│   ├── dashboards/            # Дашборды Grafana
│   └── datasources/           # Источники данных
└── nginx/
    ├── nginx.conf             # Основной конфиг Nginx
    └── nginx_status.conf      # Конфиг для метрик
```

## Добавление новых метрик

1. Добавь новый экспортер в `docker-compose.yml`
2. Настрой сбор в `prometheus/prometheus.yml`
3. Создай дашборд в `grafana/dashboards/`

## Примеры запросов PromQL

```promql
# CPU хоста
100 - (avg by (instance) (irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)

# Память хоста
(1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)) * 100

# Запросы к Nginx
rate(nginx_http_requests_total[5m])
```
