# Dockprom - Docker Monitoring with AI/LLM Observability

<div align="center">

![GitHub stars](https://img.shields.io/github/stars/waitesgithub/dockprom?style=social)
![GitHub forks](https://img.shields.io/github/forks/waitesgithub/dockprom?style=social)
![Docker Pulls](https://img.shields.io/docker/pulls/prom/prometheus?label=Prometheus%20Pulls)
![License](https://img.shields.io/github/license/waitesgithub/dockprom)

**A comprehensive monitoring and observability solution for Docker hosts, containers, and AI/LLM applications**

[Quick Start](#-quick-start) • [Features](#-features) • [AI Observability](#-ai-observability-setup) • [Documentation](#-documentation) • [Contributing](#-contributing)

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Architecture](#-architecture)
- [Quick Start](#-quick-start)
- [AI Observability Setup](#-ai-observability-setup)
- [Accessing Services](#-accessing-services)
- [Monitoring Stack Components](#-monitoring-stack-components)
- [Alerting & Notifications](#-alerting--notifications)
- [Grafana Dashboards](#-grafana-dashboards)
- [Advanced Configuration](#-advanced-configuration)
- [Troubleshooting](#-troubleshooting)
- [Documentation](#-documentation)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🎯 Overview

Dockprom is a **production-ready monitoring solution** that combines traditional infrastructure monitoring with cutting-edge AI/LLM observability. Built on the proven Prometheus + Grafana stack, it extends monitoring capabilities specifically for modern AI applications.

### What Makes This Special?

- ✅ **Drop-in AI Observability**: Monitor Ollama and LLM applications with zero code changes
- ✅ **Comprehensive Alerting**: 32 pre-configured alerts for infrastructure and AI workloads
- ✅ **Full Stack Visibility**: Metrics, logs, and traces in one unified platform
- ✅ **Production Ready**: Battle-tested components with industry best practices
- ✅ **Easy Deployment**: Single docker-compose command to get started

---

## ⭐ Features

### Infrastructure Monitoring
- 📊 **Host Metrics**: CPU, memory, disk, network monitoring via Node Exporter
- 🐳 **Container Metrics**: Docker container resource usage via cAdvisor
- ⚡ **Real-time Dashboards**: Pre-built Grafana dashboards for immediate insights
- 🔔 **Smart Alerting**: Intelligent alert routing by severity and service type

### AI/LLM Observability
- 🤖 **LLM Performance Tracking**: Latency, error rate, token usage, TTFT (Time To First Token)
- 🚪 **Ollama Gateway**: Transparent proxy with automatic tracing and metrics
- 📈 **Recording Rules**: Pre-aggregated metrics for AI workload analysis
- 🔍 **Distributed Tracing**: Full request flow visibility via Tempo
- 📝 **Log Aggregation**: Centralized logs via Loki and Grafana Alloy

### Observability Platform
- 📊 **Prometheus**: Time-series metrics database with flexible querying
- 📈 **Grafana**: Beautiful visualizations and dashboards
- 📋 **Loki**: Scalable log aggregation system
- 🔍 **Tempo**: High-scale distributed tracing backend
- 🔄 **OpenTelemetry**: Vendor-neutral instrumentation and collection

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                          Your AI Application                             │
│                    (Python, Node.js, Go, etc.)                          │
└────────────────────────────────┬────────────────────────────────────────┘
                                 │
                    ┌────────────┴────────────┐
                    │   Ollama Gateway        │  ← Zero-code AI observability
                    │   (Envoy Proxy)         │    Automatic tracing/metrics
                    └────────────┬────────────┘
                                 │
                    ┌────────────┴────────────┐
                    │   Ollama (Host)         │  ← Your LLM inference
                    │   localhost:11434       │
                    └─────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────┐
│                     Observability Pipeline                               │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                           │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐              │
│  │ OTEL         │───▶│ Prometheus   │───▶│  Grafana     │              │
│  │ Collector    │    │ (Metrics)    │    │ (Dashboards) │              │
│  └──────────────┘    └──────────────┘    └──────────────┘              │
│         │                    │                     │                     │
│         │            ┌───────┴──────────┐         │                     │
│         │            │  AlertManager    │         │                     │
│         │            │  (Notifications) │         │                     │
│         │            └──────────────────┘         │                     │
│         │                                          │                     │
│         ├────────────────────────────────────────▶│                     │
│         │                                          │                     │
│  ┌──────▼──────┐    ┌──────────────┐             │                     │
│  │   Tempo     │◀───│    Alloy     │─────────────┘                     │
│  │  (Traces)   │    │ (Log Agent)  │                                   │
│  └─────────────┘    └──────┬───────┘                                   │
│         │                   │                                            │
│         │            ┌──────▼───────┐                                   │
│         └───────────▶│     Loki     │                                   │
│                      │    (Logs)    │                                   │
│                      └──────────────┘                                   │
│                                                                           │
│  ┌──────────────┐    ┌──────────────┐                                  │
│  │ Node         │───▶│  cAdvisor    │────────────┐                     │
│  │ Exporter     │    │ (Containers) │            │                     │
│  └──────────────┘    └──────────────┘            │                     │
│                                          ┌────────▼─────────┐           │
│                                          │   Prometheus     │           │
│                                          │   (Aggregation)  │           │
│                                          └──────────────────┘           │
└─────────────────────────────────────────────────────────────────────────┘
```

**Data Flow:**
1. **AI Application** → Ollama Gateway → Ollama (LLM inference)
2. **Ollama Gateway** → OTEL Collector (traces, metrics)
3. **OTEL Collector** → Prometheus (metrics), Tempo (traces)
4. **Container Logs** → Alloy → Loki
5. **Host/Container Metrics** → Node Exporter/cAdvisor → Prometheus
6. **All Data** → Grafana (unified visualization)
7. **Alerts** → AlertManager → Slack/Teams/Mattermost

---

## 🚀 Quick Start

### Prerequisites

- Docker Engine >= 20.10
- Docker Compose >= v2.0
- 4GB RAM minimum (8GB recommended)
- Host running Ollama on `localhost:11434` (for AI observability)

### Installation

**1. Clone the repository**
```bash
git clone https://github.com/waitesgithub/dockprom.git
cd dockprom
```

**2. Start the stack**
```bash
ADMIN_USER='admin' ADMIN_PASSWORD='admin' ADMIN_PASSWORD_HASH='$2a$14$1l.IozJx7xQRVmlkEQ32OeEEfP5mRxTpbDTCTcXRqn19gXD8YK1pO' docker-compose up -d
```

**Note:** Caddy v2 requires hashed passwords. The above hash corresponds to password 'admin'. To generate a new hash:
```bash
docker run --rm caddy caddy hash-password --plaintext 'your_password'
```

**3. Verify deployment**
```bash
docker-compose ps
```

All services should show as "Up". Access Grafana at `http://localhost:3000` (admin/admin).

### First Steps

1. **Open Grafana**: Navigate to `http://localhost:3000`
2. **View Pre-built Dashboards**:
   - Docker Host Dashboard
   - Docker Containers Dashboard
   - Monitor Services Dashboard
   - AI/LLM Performance Dashboard (if AI observability is configured)
3. **Check Prometheus**: Visit `http://localhost:9090` to explore metrics
4. **Configure Alerts**: Follow [Alerting Setup](#alerting--notifications)

---

## 🤖 AI Observability Setup

Monitor your AI/LLM applications with zero code changes using the Ollama Gateway.

### Step 1: Configure Host Ollama

Ensure Ollama is accessible from Docker containers:

```bash
# For Linux, Ollama must listen on a non-loopback interface
export OLLAMA_HOST=0.0.0.0:11434
ollama serve

# Verify it's running
curl http://localhost:11434/api/tags
```

### Step 2: Use Ollama Gateway (Zero-Code Option)

**Instead of connecting to `localhost:11434`, connect to the gateway at port `8080`:**

```python
# Before (direct Ollama connection)
client = OpenAI(base_url="http://localhost:11434/v1")

# After (with observability)
client = OpenAI(base_url="http://localhost:8080/v1")
```

The gateway automatically:
- ✅ Captures all requests/responses as traces
- ✅ Measures Time To First Token (TTFT)
- ✅ Tracks token usage and throughput
- ✅ Monitors error rates and latency
- ✅ Forwards requests to Ollama unchanged

### Step 3: Instrument Your Application (Full Observability)

For custom metrics and traces, instrument with OpenTelemetry:

**Python Example:**
```python
from opentelemetry import trace
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor
from opentelemetry.exporter.otlp.proto.grpc.trace_exporter import OTLPSpanExporter

# Configure OTLP exporter
provider = TracerProvider()
processor = BatchSpanProcessor(
    OTLPSpanExporter(endpoint="http://localhost:4317", insecure=True)
)
provider.add_span_processor(processor)
trace.set_tracer_provider(provider)

# Use in your code
tracer = trace.get_tracer(__name__)

with tracer.start_as_current_span("llm_request"):
    response = client.chat.completions.create(
        model="llama2",
        messages=[{"role": "user", "content": "Hello!"}]
    )
```

**Node.js Example:**
```javascript
const { NodeTracerProvider } = require('@opentelemetry/sdk-trace-node');
const { OTLPTraceExporter } = require('@opentelemetry/exporter-trace-otlp-grpc');
const { BatchSpanProcessor } = require('@opentelemetry/sdk-trace-base');

const provider = new NodeTracerProvider();
provider.addSpanProcessor(
  new BatchSpanProcessor(
    new OTLPTraceExporter({
      url: 'http://localhost:4317'
    })
  )
);
provider.register();
```

### Step 4: View Your Data

**Grafana Dashboards:**
- AI/LLM Performance Dashboard: `http://localhost:3000`
- Distributed Traces: Navigate to **Explore** → Select **Tempo** datasource
- Logs: Navigate to **Explore** → Select **Loki** datasource

**Query Examples:**

```promql
# Average LLM latency (PromQL)
ai:llm:latency_p95_ms

# Error rate
ai:llm:error_rate

# Time to first token
ai:llm:ttft_p95_ms

# Throughput
ai:llm:throughput_rpm
```

---

## 🌐 Accessing Services

| Service | URL | Credentials | Purpose |
|---------|-----|-------------|---------|
| **Grafana** | http://localhost:3000 | admin/admin | Dashboards & visualization |
| **Prometheus** | http://localhost:9090 | admin/admin (via Caddy) | Metrics & queries |
| **AlertManager** | http://localhost:9093 | admin/admin (via Caddy) | Alert management |
| **Loki** | http://localhost:3100 | admin/admin (via Caddy) | Log aggregation API |
| **Tempo** | http://localhost:3200 | admin/admin (via Caddy) | Distributed tracing API |
| **OTEL Collector** | http://localhost:4317 | - | OTLP gRPC endpoint |
| **Ollama Gateway** | http://localhost:8080 | - | AI observability proxy |
| **cAdvisor** | http://localhost:8081 | - | Container metrics UI |
| **Node Exporter** | http://localhost:9100 | - | Host metrics API |
| **Pushgateway** | http://localhost:9091 | admin/admin (via Caddy) | Batch job metrics |

**Default Credentials:**
- Grafana: `admin` / `admin` (change on first login)
- Caddy protected services: `admin` / `admin` (configure via environment variables)

---

## 📦 Monitoring Stack Components

### Core Monitoring

#### Prometheus
**Purpose:** Time-series metrics database and alerting engine

**Features:**
- Multi-dimensional data model with time series
- Flexible query language (PromQL)
- Pull-based metrics collection
- Service discovery and target monitoring
- Built-in alerting rules

**Configuration:** `prometheus/prometheus.yml`

#### Grafana
**Purpose:** Analytics and monitoring visualization platform

**Features:**
- Interactive dashboards with drill-down capabilities
- Multiple datasource support (Prometheus, Loki, Tempo)
- Alert visualization and management
- User authentication and RBAC
- Dashboard provisioning and sharing

**Configuration:** `grafana/provisioning/`

#### Node Exporter
**Purpose:** Host-level metrics exporter

**Metrics:**
- CPU usage, load average
- Memory and swap utilization
- Disk I/O, space usage
- Network statistics
- System uptime and processes

#### cAdvisor
**Purpose:** Container resource usage and performance metrics

**Metrics:**
- Container CPU, memory, network, disk usage
- Per-container resource limits
- Historical resource usage
- Docker container lifecycle events

### AI/LLM Observability

#### Ollama Gateway (Envoy Proxy)
**Purpose:** Transparent proxy for AI observability

**Features:**
- Zero-code instrumentation for Ollama requests
- Automatic distributed tracing via OpenTelemetry
- Request/response capture with span attributes
- Performance metrics (latency, throughput)
- Error tracking and rate limiting

**Port:** 8080 (proxies to host.docker.internal:11434)

#### OpenTelemetry Collector
**Purpose:** Vendor-neutral telemetry data pipeline

**Features:**
- OTLP protocol receiver (gRPC and HTTP)
- Data processing and enrichment
- Export to multiple backends (Prometheus, Tempo, Loki)
- Trace sampling and filtering
- Metrics aggregation

**Configuration:** `otel-collector/config.yaml`

#### Tempo
**Purpose:** High-scale distributed tracing backend

**Features:**
- Cost-effective trace storage
- Integration with Loki for trace-to-log correlation
- TraceQL for querying traces
- Compatible with OpenTelemetry, Jaeger, Zipkin
- No indexing required (search by trace ID)

**Configuration:** `tempo/tempo.yaml`

#### Loki
**Purpose:** Log aggregation and querying system

**Features:**
- Label-based log indexing (like Prometheus for logs)
- LogQL query language
- Integration with Grafana for log visualization
- Cost-effective storage (object storage compatible)
- Multi-tenancy support

**Configuration:** `loki/loki-config.yaml`

#### Grafana Alloy
**Purpose:** Lightweight log collection agent

**Features:**
- Docker container log collection
- Log parsing and enrichment
- Forwarding to Loki
- Low resource footprint
- Dynamic configuration reload

### Alerting & Notifications

#### AlertManager
**Purpose:** Alert routing, grouping, and notification management

**Features:**
- Intelligent alert grouping and deduplication
- Multiple notification channels (Slack, Teams, Mattermost, email)
- Alert silencing and inhibition
- Routing based on labels and severity
- Template-based notifications

**Configuration:** `alertmanager/config.yml`

#### Pushgateway
**Purpose:** Metrics gateway for batch jobs and short-lived processes

**Features:**
- Accept metrics pushed from batch jobs
- Preserve metrics for Prometheus scraping
- Support for multiple job metrics
- Metric expiration and cleanup

---

## 🔔 Alerting & Notifications

### Alert Coverage

This stack includes **32 pre-configured alerts** covering all critical aspects:

| Category | # Alerts | Severity Levels | Examples |
|----------|----------|-----------------|----------|
| **Service Health** | 2 | Critical | ServiceDown, ObservabilityStackDown |
| **Infrastructure** | 6 | Warning, Critical | HighCPULoad, CriticalMemoryUsage, HighDiskUsage |
| **AI/LLM Performance** | 5 | Warning, Critical | HighLLMErrorRate, SlowTimeToFirstToken, HighLLMLatency |
| **Ollama Gateway** | 4 | Warning, Critical | HighGateway5xxRate, GatewayHighLatency |
| **RAG Tools** | 4 | Warning | HighRAGRetrievalLatency, LowRAGRelevanceScore |
| **AI Safety & Security** | 3 | Critical | ContentModerationSpikeDetected, UnusualModelAccessPattern |
| **Observability Stack** | 5 | Warning, Critical | PrometheusHighMemory, LokiHighIngestionRate |
| **Container Health** | 3 | Warning, Critical | ContainerHighMemory, ContainerHighCPU |

### Notification Channels

**Supported Integrations:**
- 💬 **Slack**: Real-time notifications to channels
- 👥 **Microsoft Teams**: Webhook-based alerts with rich formatting
- 💻 **Mattermost**: Slack-compatible webhook integration
- 📧 **Email**: SMTP-based notifications (configure in AlertManager)

### Alert Routing Strategy

```yaml
# Critical alerts → Immediate notification (10s wait, 1m interval, 4h repeat)
# Warning alerts → Standard notification (30s wait, 5m interval, 12h repeat)

Routes:
├─ Critical Alerts (severity=critical)
│  ├─ AI/LLM Critical → ai-llm-critical receiver
│  ├─ Infrastructure Critical → infrastructure-critical receiver
│  └─ Security Critical → security-critical receiver
│
└─ Warning Alerts (severity=warning)
   ├─ AI/LLM Warnings → ai-llm-warnings receiver
   └─ Infrastructure Warnings → infrastructure-warnings receiver
```

**Inhibit Rules:** Critical alerts suppress related warnings to prevent alert fatigue.

### Configuration Files

- **Alert Rules**: `prometheus/alert.rules` (32 alerts)
- **AlertManager Config**: `alertmanager/config.yml` (routing & receivers)
- **Recording Rules**: `prometheus/recording.rules` (pre-aggregated AI metrics)

**Setup Guide:** See [WEBHOOK_SETUP.md](./WEBHOOK_SETUP.md) for webhook configuration
**Runbooks:** See [ALERT_RUNBOOKS.md](./ALERT_RUNBOOKS.md) for troubleshooting steps

---

## 📊 Grafana Dashboards

### Pre-built Dashboards

#### 1. Docker Host Dashboard
**Metrics:**
- CPU usage (overall, per-core)
- Memory utilization and swap
- Disk I/O, space usage, inode usage
- Network traffic (RX/TX)
- System load and uptime

**Note:** Update `fstype` in dashboard JSON based on your filesystem (e.g., `ext4`, `xfs`, `btrfs`).

#### 2. Docker Containers Dashboard
**Metrics:**
- Per-container CPU usage
- Memory usage and limits
- Network I/O by container
- Container lifecycle events
- Resource quotas and throttling

#### 3. Monitor Services Dashboard
**Metrics:**
- Service availability (up/down status)
- Scrape duration and success rate
- Target health across all jobs
- Prometheus internal metrics

#### 4. AI/LLM Performance Dashboard
**Metrics:**
- LLM request rate and error rate
- Latency percentiles (p50, p95, p99)
- Time To First Token (TTFT) tracking
- Token usage and throughput
- Model-specific performance breakdown

#### 5. Alert Monitoring Dashboard
**Panels:**
- Critical vs warning alert counts
- Alert firing rate over time
- Active alerts table with details
- Alerts by service/category
- Alert pattern heatmap

**Import Guide:** See [GRAFANA_DASHBOARD_SETUP.md](./GRAFANA_DASHBOARD_SETUP.md)

### Dashboard JSON Files

Pre-configured dashboard JSON files are available in `grafana/provisioning/dashboards/`:
- `docker-host-dashboard.json`
- `docker-containers-dashboard.json`
- `monitor-services-dashboard.json`
- `ai-llm-dashboard.json`
- `alert-monitoring-dashboard.json`

**Auto-provisioning:** Dashboards are automatically loaded on Grafana startup.

---

## ⚙️ Advanced Configuration

### Custom Recording Rules

Add custom pre-aggregated metrics for AI workloads in `prometheus/recording.rules`:

```yaml
groups:
  - name: custom_ai_metrics
    interval: 30s
    rules:
      # Custom model-specific error rate
      - record: ai:llm:model_error_rate:5m
        expr: |
          sum by (model) (rate(llm_request_errors_total[5m]))
          / sum by (model) (rate(llm_requests_total[5m]))

      # Average tokens per request
      - record: ai:llm:avg_tokens_per_request
        expr: |
          sum(rate(llm_tokens_total[5m]))
          / sum(rate(llm_requests_total[5m]))
```

### Environment Variables

Configure stack behavior via environment variables:

```bash
# Prometheus
PROMETHEUS_RETENTION=15d

# Grafana
ADMIN_USER=admin
ADMIN_PASSWORD=your_secure_password
ADMIN_PASSWORD_HASH=$(docker run --rm caddy caddy hash-password --plaintext 'your_secure_password')

# Grafana additional settings
GF_SERVER_ROOT_URL=https://grafana.yourdomain.com
GF_AUTH_ANONYMOUS_ENABLED=false
```

### Scaling Considerations

**For Production Deployments:**

1. **Persistent Storage:**
   ```yaml
   volumes:
     - prometheus-data:/prometheus
     - grafana-data:/var/lib/grafana
     - loki-data:/loki
   ```

2. **Resource Limits:**
   ```yaml
   deploy:
     resources:
       limits:
         cpus: '2'
         memory: 4G
       reservations:
         memory: 2G
   ```

3. **High Availability:**
   - Run multiple AlertManager instances with clustering
   - Use external Prometheus remote storage (Thanos, Cortex)
   - Configure Loki with object storage backend (S3, GCS)

### Security Hardening

**1. Enable Authentication:**
```yaml
# Prometheus (via Caddy)
# Already configured with basic auth

# Grafana
GF_AUTH_ANONYMOUS_ENABLED=false
GF_SECURITY_ADMIN_PASSWORD=<strong_password>
```

**2. Network Isolation:**
```yaml
networks:
  monitor-net:
    driver: bridge
    internal: false  # Set to true for isolation (requires external proxy)
```

**3. TLS Encryption:**
Caddy automatically handles TLS. Configure your domain in the Caddyfile:
```
https://grafana.yourdomain.com {
    reverse_proxy grafana:3000
    basicauth /* {
        {$ADMIN_USER} {$ADMIN_PASSWORD_HASH}
    }
}
```

### Custom Alert Rules

Add your own alerts in `prometheus/alert.rules`:

```yaml
groups:
  - name: custom_alerts
    rules:
      - alert: CustomHighErrorRate
        expr: rate(http_requests_total{status=~"5.."}[5m]) > 0.05
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "High error rate detected"
          description: "Error rate is {{ $value | humanizePercentage }}"
          runbook_url: "https://github.com/waitesgithub/dockprom/blob/master/docs/runbooks/CustomHighErrorRate.md"
```

---

## 🔧 Troubleshooting

### Common Issues

#### 1. Services Not Starting

**Symptom:** Containers exit immediately or show "unhealthy" status

**Solutions:**
```bash
# Check logs
docker-compose logs <service_name>

# Common issues:
# - Port conflicts: Change ports in docker-compose.yml
# - Permission issues: Check volume mount permissions
# - Resource constraints: Increase Docker memory limit
```

#### 2. Ollama Gateway Can't Connect to Host

**Symptom:** Gateway returns 503 errors or connection refused

**Solutions:**
```bash
# Verify Ollama is running and accessible
curl http://localhost:11434/api/tags

# For Linux, ensure Ollama listens on 0.0.0.0
export OLLAMA_HOST=0.0.0.0:11434
ollama serve

# Check gateway can reach host
docker exec ollama-gateway curl http://host.docker.internal:11434/api/tags

# Update docker-compose.yml if needed:
extra_hosts:
  - "host.docker.internal:host-gateway"
```

#### 3. No Metrics in Grafana

**Symptom:** Dashboards show "No data" or empty graphs

**Solutions:**
```bash
# Verify Prometheus is scraping targets
# Visit http://localhost:9090/targets

# Check Prometheus logs
docker-compose logs prometheus

# Verify datasource configuration in Grafana
# Settings → Data Sources → Prometheus
# URL should be: http://prometheus:9090
```

#### 4. Alerts Not Firing

**Symptom:** Expected alerts don't appear in AlertManager

**Solutions:**
```bash
# Validate alert rules syntax
docker exec prometheus promtool check rules /etc/prometheus/alert.rules

# Check if rules are loaded
curl http://admin:admin@localhost:9090/api/v1/rules

# Verify AlertManager is receiving alerts
curl http://admin:admin@localhost:9093/api/v1/alerts

# Reload Prometheus configuration
curl -X POST http://admin:admin@localhost:9090/-/reload
```

#### 5. High Memory Usage

**Symptom:** Prometheus or Loki consuming excessive memory

**Solutions:**
```yaml
# Adjust retention periods in prometheus/prometheus.yml
global:
  scrape_interval: 15s

# Add storage configuration
storage:
  tsdb:
    retention.time: 15d  # Reduce from default 30d
    retention.size: 10GB  # Add size-based retention

# For Loki, adjust in loki/loki-config.yaml
limits_config:
  retention_period: 168h  # 7 days
```

#### 6. Traces Not Appearing in Tempo

**Symptom:** No traces visible in Grafana Explore → Tempo

**Solutions:**
```bash
# Verify OTEL Collector is receiving traces
docker-compose logs otel-collector | grep -i trace

# Check Tempo ingestion
curl http://admin:admin@localhost:3200/api/search

# Verify application is sending to correct endpoint
# OTLP gRPC: localhost:4317
# OTLP HTTP: localhost:4318

# Check trace sampling (might be sampling out spans)
```

#### 7. Filesystem Type Issues

**Symptom:** Disk metrics showing "No data" in dashboards

**Solution:**
```bash
# Find your filesystem type
node_filesystem_free_bytes

# Update grafana dashboard JSON files:
# Replace fstype="aufs" with your actual filesystem type
# Common types: ext4, xfs, btrfs, overlay2

# Example in docker_host.json:
"expr": "sum(node_filesystem_free_bytes{fstype=\"ext4\"})"
```

### Verification Commands

```bash
# Check all services are running
docker-compose ps

# View service logs
docker-compose logs -f <service_name>

# Test Prometheus targets
curl http://admin:admin@localhost:9090/api/v1/targets | jq .

# Test AlertManager configuration
docker exec alertmanager amtool check-config /etc/alertmanager/config.yml

# Verify Grafana datasources
curl -u admin:admin http://localhost:3000/api/datasources

# Test OTLP endpoint
grpcurl -plaintext -d '{}' localhost:4317 list
```

### Performance Optimization

**1. Reduce Metrics Cardinality:**
```yaml
# Drop unnecessary labels in prometheus/prometheus.yml
metric_relabel_configs:
  - source_labels: [__name__]
    regex: 'high_cardinality_metric.*'
    action: drop
```

**2. Adjust Scrape Intervals:**
```yaml
# For less critical metrics, increase interval
scrape_configs:
  - job_name: 'low-priority-job'
    scrape_interval: 60s  # Instead of default 15s
```

**3. Enable Query Caching:**
```yaml
# In grafana/grafana.ini
[caching]
enabled = true
```

---

## 📚 Documentation

### Quick Links

- **Alert Rules Reference**: [alert.rules](./prometheus/alert.rules)
- **Recording Rules**: [recording.rules](./prometheus/recording.rules)
- **Prometheus Config**: [prometheus.yml](./prometheus/prometheus.yml)
- **AlertManager Config**: [alertmanager/config.yml](./alertmanager/config.yml)
- **Docker Compose**: [docker-compose.yml](./docker-compose.yml)

### Implementation Guides

- **[IMPLEMENTATION_SUMMARY.md](./IMPLEMENTATION_SUMMARY.md)** - Complete deployment checklist
- **[WEBHOOK_SETUP.md](./WEBHOOK_SETUP.md)** - Teams & Mattermost webhook configuration
- **[ALERT_RUNBOOKS.md](./ALERT_RUNBOOKS.md)** - Troubleshooting guide for each alert
- **[GRAFANA_DASHBOARD_SETUP.md](./GRAFANA_DASHBOARD_SETUP.md)** - Dashboard creation guide

### External Resources

- [Prometheus Documentation](https://prometheus.io/docs/)
- [Grafana Documentation](https://grafana.com/docs/)
- [OpenTelemetry Collector](https://opentelemetry.io/docs/collector/)
- [Loki Documentation](https://grafana.com/docs/loki/)
- [Tempo Documentation](https://grafana.com/docs/tempo/)
- [Ollama Documentation](https://github.com/ollama/ollama/tree/main/docs)

---

## 🤝 Contributing

We welcome contributions! Here's how you can help:

### Reporting Issues

1. Check existing [issues](https://github.com/waitesgithub/dockprom/issues)
2. Create detailed bug reports with:
   - Dockprom version
   - Docker version
   - Steps to reproduce
   - Expected vs actual behavior
   - Relevant logs

### Submitting Changes

1. **Fork the repository**
2. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. **Make your changes**
   - Follow existing code style
   - Update documentation
   - Add tests if applicable
4. **Commit with clear messages**
   ```bash
   git commit -m "feat: add support for new LLM metrics"
   ```
5. **Push and create a Pull Request**
   ```bash
   git push origin feature/your-feature-name
   ```

### Development Setup

```bash
# Clone your fork
git clone https://github.com/YOUR_USERNAME/dockprom.git
cd dockprom

# Add upstream remote
git remote add upstream https://github.com/waitesgithub/dockprom.git

# Start development environment
docker-compose up -d

# Make changes and test
docker-compose restart <service_name>
```

### Areas for Contribution

- 📊 New Grafana dashboards for specific use cases
- 🔔 Additional alert rules and runbooks
- 🤖 Support for more LLM providers (OpenAI, Anthropic, etc.)
- 📝 Documentation improvements and translations
- 🧪 Test coverage and CI/CD improvements
- 🔧 Performance optimizations

### Code of Conduct

- Be respectful and inclusive
- Provide constructive feedback
- Focus on what is best for the community
- Show empathy towards other community members

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](./LICENSE) file for details.

### MIT License Summary

- ✅ Commercial use
- ✅ Modification
- ✅ Distribution
- ✅ Private use
- ⚠️ Liability and warranty disclaimers apply

---

## 🙏 Acknowledgments

This project builds upon the excellent work of:

- **Stefan Prodan** - Original [dockprom](https://github.com/stefanprodan/dockprom) creator
- **Prometheus Team** - For the amazing monitoring system
- **Grafana Labs** - For Grafana, Loki, Tempo, and Alloy
- **OpenTelemetry Community** - For vendor-neutral observability standards
- **Ollama Team** - For accessible local LLM deployment
- **Docker Community** - For containerization excellence

### Special Thanks

- All contributors who have helped improve this project
- The DevOps and AI/ML communities for feedback and testing
- Open source maintainers whose tools make this possible

---

## 📊 Project Stats

![GitHub last commit](https://img.shields.io/github/last-commit/waitesgithub/dockprom)
![GitHub commit activity](https://img.shields.io/github/commit-activity/m/waitesgithub/dockprom)
![GitHub contributors](https://img.shields.io/github/contributors/waitesgithub/dockprom)
![Docker Image Size](https://img.shields.io/docker/image-size/prom/prometheus)

---

## 🔗 Related Projects

- [Netdata](https://github.com/netdata/netdata) - Real-time performance monitoring
- [VictoriaMetrics](https://github.com/VictoriaMetrics/VictoriaMetrics) - High-performance Prometheus alternative
- [Thanos](https://github.com/thanos-io/thanos) - Prometheus long-term storage
- [Cortex](https://github.com/cortexproject/cortex) - Horizontally scalable Prometheus
- [OpenLLMetry](https://github.com/traceloop/openllmetry) - OpenTelemetry for LLMs

---

## 🗺️ Roadmap

### Planned Features

- [ ] **Multi-LLM Provider Support**: OpenAI, Anthropic, Cohere gateway adapters
- [ ] **Cost Tracking Dashboard**: Token costs and usage analytics
- [ ] **Automated Performance Testing**: Load testing framework for LLMs
- [ ] **Enhanced Security Monitoring**: Content moderation dashboards
- [ ] **Mobile Alerts**: Push notifications via mobile apps
- [ ] **GitOps Integration**: Flux/ArgoCD deployment examples
- [ ] **Kubernetes Support**: Helm charts and operator
- [ ] **Advanced RAG Metrics**: Vector database performance tracking

### Recently Added

- ✅ Comprehensive alert rules (32 alerts)
- ✅ Multi-channel notifications (Slack, Teams, Mattermost)
- ✅ AI/LLM observability with Ollama Gateway
- ✅ Distributed tracing with Tempo
- ✅ Log aggregation with Loki and Alloy
- ✅ Pre-aggregated AI metrics via recording rules

---

## 💬 Community & Support

### Getting Help

- **Documentation**: Check this README and documentation files
- **Issues**: [GitHub Issues](https://github.com/waitesgithub/dockprom/issues)
- **Discussions**: [GitHub Discussions](https://github.com/waitesgithub/dockprom/discussions)

### Stay Updated

- ⭐ **Star this repo** to stay notified of updates
- 👁️ **Watch releases** for new versions
- 🔄 **Follow the project** for development updates

---

<div align="center">

## 🌟 Show Your Support

If you find this project useful, please consider:

- ⭐ **Starring the repository**
- 🔄 **Sharing with your network**
- 🐛 **Reporting bugs and issues**
- 💡 **Suggesting new features**
- 🤝 **Contributing code or documentation**

---

**Built with ❤️ for the DevOps and AI/ML communities**

*Making observability accessible for everyone*

---

**Questions?** Open an [issue](https://github.com/waitesgithub/dockprom/issues) • **Want to contribute?** Check our [contributing guidelines](#-contributing)

</div>
