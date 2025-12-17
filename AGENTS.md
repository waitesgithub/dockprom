# dockprom-ai (project instructions)

## What this repo is
This repo is **Dockprom + AI observability**:
- **Metrics**: Prometheus scrapes exporters, Envoy gateway, and the OTel collector Prometheus exporter.
- **Logs**: Docker container logs → **Alloy** → **Loki**.
- **Traces**: OTLP traces → **otel-collector** → **Tempo**.
- **LLM gateway**: **Envoy** (`ollama-gateway`) proxies host Ollama and emits request metrics/traces.

## How to run (dev)
- Start stack: `docker compose up -d`
- Core entrypoints:
  - Grafana: `http://<host-ip>:3000`
  - Prometheus: `http://<host-ip>:9090`
  - OTel Collector (OTLP): `http://<host-ip>:4318` and `<host-ip>:4317`
  - Ollama gateway: `http://<host-ip>:11435`

## Guardrails (security + operability)
- **No secrets in git**: never commit API keys, Slack webhooks, or credentials. Use environment variables or local `.env` (ignored).
- **Do not remove auth**: do not weaken `caddy/Caddyfile` basic auth or expose new unauthenticated admin endpoints.
- **Don’t widen ports casually**: be explicit if you bind services to `0.0.0.0` or add new published ports.
- **Be careful with Docker logs**: Alloy ships Docker logs to Loki. Avoid logging prompts/responses or secrets from any agent tool.

## Repo editing conventions
- Prefer small, reviewable changes.
- For config edits, keep existing formatting and comments.
- If you change observability pipelines, keep the end-to-end flow working:
  - OTLP → `otel-collector` → Tempo/Loki/Prometheus exporter.

## Useful files
- `docker-compose.yml`: service graph and ports
- `otel-collector/config.yml`: OTLP ingest + export pipelines
- `envoy/envoy.yml`: Ollama gateway proxy + tracing
- `alloy/config.alloy`: Docker logs → Loki
- `prometheus/prometheus.yml`: scrape targets
- `grafana/provisioning/*`: dashboards and datasources
