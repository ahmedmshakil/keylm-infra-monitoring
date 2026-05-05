# KeyLM Infra Monitoring

Prometheus + Grafana + Node Exporter stack for KeyLM, intended to run on an Azure Ubuntu VM with Docker Compose.

KeyLM app metrics are scraped from:

```text
https://keylm.shakilahmed.tech/api/metrics
```

The app endpoint requires:

```http
Authorization: Bearer <METRICS_TOKEN>
```

The real token must stay out of git.

## Services

- Prometheus: scrapes KeyLM app metrics and Azure VM node metrics.
- Grafana: dashboard UI with Prometheus provisioned as the default datasource.
- Node Exporter: exposes Azure VM host metrics to Prometheus over the private Docker network.

## Setup

Copy the example environment file:

```bash
cp .env.example .env
```

Edit `.env` and set a strong Grafana password:

```env
GRAFANA_ADMIN_USER=admin
GRAFANA_ADMIN_PASSWORD=replace-with-a-strong-password
GRAFANA_PORT=3000
GRAFANA_ROOT_URL=http://localhost:3000/
PROMETHEUS_PORT=9090
PROMETHEUS_RETENTION=30d
KEYLM_METRICS_TOKEN_FILE=./secrets/keylm_metrics_token
```

Create the local secret file for the KeyLM metrics token:

```bash
mkdir -p secrets
printf '%s' 'paste-your-real-metrics-token-here' > secrets/keylm_metrics_token
chmod 600 secrets/keylm_metrics_token
```

Do not commit `.env` or anything under `secrets/`.

## Start

Validate the Compose file:

```bash
docker compose config
```

Start the stack:

```bash
docker compose up -d
```

Check containers:

```bash
docker compose ps
```

## Verify Prometheus

Prometheus is bound to localhost by default:

```text
http://127.0.0.1:9090
```

If you are connecting from your laptop, use an SSH tunnel:

```bash
ssh -L 9090:127.0.0.1:9090 azureuser@YOUR_VM_PUBLIC_IP
```

Open:

```text
http://127.0.0.1:9090/targets
```

Expected targets:

- `keylm-app`: `UP`
- `azure-vm-node`: `UP`

Useful Prometheus queries:

```promql
up
keylm_http_requests_total
node_cpu_seconds_total
```

If `keylm-app` is down, check:

- `secrets/keylm_metrics_token` contains the same token configured as `METRICS_TOKEN` in Vercel.
- The KeyLM app is deployed and `/api/metrics` is reachable over HTTPS.
- The Azure VM can reach `keylm.shakilahmed.tech`.

## Verify Grafana

Grafana is bound to localhost by default:

```text
http://127.0.0.1:3000
```

Use an SSH tunnel from your laptop:

```bash
ssh -L 3000:127.0.0.1:3000 azureuser@YOUR_VM_PUBLIC_IP
```

Open:

```text
http://127.0.0.1:3000
```

Log in with:

- Username: `GRAFANA_ADMIN_USER`
- Password: `GRAFANA_ADMIN_PASSWORD`

Prometheus is provisioned as the default datasource. The `KeyLM Overview` dashboard is provisioned under the `KeyLM` folder.

To verify the datasource manually, open Grafana Explore and run:

```promql
up
```

## Change Prometheus Retention

Default retention is `30d`.

To change it, update `.env`:

```env
PROMETHEUS_RETENTION=60d
```

Then restart Prometheus:

```bash
docker compose up -d prometheus
```

## Production Exposure

Do not expose Prometheus or Node Exporter publicly.

Current bindings:

- Prometheus: `127.0.0.1:9090`
- Grafana: `127.0.0.1:3000`
- Node Exporter: no host port mapping

Later, expose only Grafana through Nginx or Caddy:

```text
dashboard.keylm.shakilahmed.tech -> 127.0.0.1:3000
```

Use HTTPS with Let's Encrypt. Keep Grafana login enabled. Do not enable anonymous Grafana access.

## Azure Firewall And NSG Recommendations

Inbound rules:

- Allow SSH `22/tcp` only from trusted admin IP addresses.
- Allow `80/tcp` and `443/tcp` only when Nginx/Caddy is ready for HTTPS.
- Do not allow public inbound `3000/tcp`.
- Do not allow public inbound `9090/tcp`.
- Do not allow public inbound `9100/tcp`.

Outbound rules:

- Allow HTTPS `443/tcp` so Prometheus can scrape `keylm.shakilahmed.tech`.
- Allow image pulls from Docker registries during setup and upgrades.

## Operations

View logs:

```bash
docker compose logs -f prometheus
docker compose logs -f grafana
docker compose logs -f node-exporter
```

Stop the stack:

```bash
docker compose down
```

Stop and remove volumes only when you want to delete stored Prometheus/Grafana data:

```bash
docker compose down -v
```
