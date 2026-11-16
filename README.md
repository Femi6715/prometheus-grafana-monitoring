# Monitoring Stack: Prometheus, Grafana and Node Exporter

A monitoring and alerting stack that runs with Docker Compose. Prometheus collects host metrics from Node Exporter, an alert rule flags high CPU usage, and Grafana visualizes the data.

## Services

| Service | Image | Port | Role |
|---|---|---|---|
| Prometheus | `prom/prometheus:v2.55.0` | 9090 | Scrapes and stores metrics, evaluates alert rules |
| Node Exporter | `prom/node-exporter:v1.8.1` | 9100 | Exposes host CPU, memory, disk and network metrics |
| Grafana | `grafana/grafana:11.1.0` | 3000 | Dashboards |

## Alert rule

`HighCPUUsage` (in `prometheus/alerts.yml`) fires when average CPU usage stays above 80% for more than 2 minutes:

```yaml
expr: avg(rate(node_cpu_seconds_total{mode!="idle"}[2m])) * 100 > 80
for: 2m
```

## Quick start

```bash
docker compose up -d
```

- Prometheus: http://localhost:9090 (check **Status > Targets** and **Alerts**)
- Grafana: http://localhost:3000 (user `admin`, password `admin`; change it on first login)

To build a dashboard in Grafana, add Prometheus as a data source with the URL `http://prometheus:9090`, then import the "Node Exporter Full" dashboard (ID 1860).

Stop the stack:

```bash
docker compose down
```

## Files

| File | Purpose |
|---|---|
| `docker-compose.yml` | The three services |
| `prometheus/prometheus.yml` | Scrape interval (15s), scrape targets and rule files |
| `prometheus/alerts.yml` | Alert rules |
