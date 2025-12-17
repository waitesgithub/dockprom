# dockprom-ai (Gemini CLI context)

## What this repo does
Runs a Docker monitoring + AI/LLM observability stack (Prometheus/Grafana/Loki/Tempo/OTel Collector + Envoy Ollama gateway).

## Non-negotiables
- Never commit secrets (API keys, tokens, webhook URLs). Use env vars or local `.env`.
- Don’t weaken or remove basic auth in `caddy/Caddyfile`.
- Be careful with logs: Alloy ships Docker logs to Loki; avoid logging prompts/responses/secrets.

## Common tasks
- Update dashboards: edit JSON in `grafana/provisioning/dashboards/`.
- Update scrape targets: edit `prometheus/prometheus.yml`.
- Update OTLP routing: edit `otel-collector/config.yml`.
- Update the Ollama gateway: edit `envoy/envoy.yml`.

## Quick run
- `docker compose up -d`
- Grafana at `http://<host-ip>:3000`
