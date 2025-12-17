# dockprom-ai (OpenCode / Crush memory)

## Repo purpose
Monitoring + observability stack for Docker, extended with AI/LLM observability:
- OTLP ingest via `otel-collector` → Tempo/Loki/Prometheus
- Docker logs via Alloy → Loki
- Envoy `ollama-gateway` provides request traces + Prometheus metrics for Ollama traffic

## Rules
- Don’t add secrets to files or output. Keep API keys in environment variables.
- Don’t weaken `caddy/Caddyfile` auth.
- Treat `docker-compose.yml` as production-like infra: small diffs, explicit security tradeoffs.

## Start/verify
- Start: `docker compose up -d`
- Grafana: `http://<host-ip>:3000`
- Prometheus: `http://<host-ip>:9090`
- OTLP HTTP: `http://<host-ip>:4318`
- Gateway: `http://<host-ip>:11435`
