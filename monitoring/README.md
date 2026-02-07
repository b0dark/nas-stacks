# Monitoring stack (Prometheus + Grafana + UniFi)

Overview
- This folder contains a Docker Compose scaffold to run Prometheus, Grafana, node-exporter, and cAdvisor on your Synology NAS. It includes a placeholder service for `unifi-poller` (UniFi metrics) — fill credentials in `.env` before starting.

Quick start
1. Copy `.env.example` to `.env` and update values (set a strong `GF_ADMIN_PASSWORD` and UniFi credentials if used).
2. From your development machine (uses the NAS Docker context):

```bash
DOCKER_API_VERSION=1.43 docker --context nas compose -f ~/Development/nas-stacks/monitoring/docker-compose.yml up -d
```

3. Grafana will be available on port `3000` of your NAS host. Prometheus UI on `9090`.

Notes & next steps
- This scaffold uses `network_mode: host` so Prometheus can scrape host-local exporters at `localhost`.
- If your UniFi controller is remote or uses a non-standard port, update the `unifi-poller` configuration and `prometheus/prometheus.yml` accordingly.
- Recommended additions: Alertmanager, backup of Prometheus/Grafana volumes, and import community Grafana dashboards for UniFi and Docker.
