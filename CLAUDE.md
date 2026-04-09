# dockprom-ai (Claude Code instructions)

## Project summary
This repo runs a Docker monitoring/observability stack:
- Prometheus + Alertmanager
- Grafana provisioning (datasources + dashboards)
- Loki (logs) via Alloy (Docker log collection)
- Tempo (traces)
- OpenTelemetry Collector (OTLP ingest)
- Envoy `ollama-gateway` (proxy to host Ollama + tracing/metrics)

## Safety rules (must-follow)
- Do **not** add or commit secrets (API keys, tokens, Slack webhook URLs). Use env vars or local `.env`.
- Do **not** remove or weaken auth in `caddy/Caddyfile`.
- Prefer least-privilege changes in `docker-compose.yml` (avoid privileged containers unless required).
- Avoid printing sensitive data into logs; Alloy ships Docker logs to Loki.

## How to verify changes
- Config-only edits: ensure YAML/Alloy syntax remains valid.
- When altering pipelines:
  - OTel collector still exposes `:8889` for Prometheus scraping.
  - Traces still reach Tempo; logs still reach Loki.

## Key entrypoints
- Grafana: `http://<host-ip>:3000`
- Prometheus: `http://<host-ip>:9090`
- OTLP HTTP: `http://<host-ip>:4318`
- Ollama gateway: `http://<host-ip>:11435`

(If you need broader repo guidance, also read `AGENTS.md`.)
