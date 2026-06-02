# DevOps & Production Infrastructure: Set 6
## Observability: Monitoring, Logging, Tracing & Incident Response

## 📘 10 Conceptual Questions
1. **The Three Pillars of Observability:** How do Metrics, Logs, and Traces complement each other? If Metrics tell you *that* something is wrong, and Logs tell you *why*, what exactly do Traces tell you?
2. **SLIs, SLOs, and SLAs:** What is the difference between a Service Level Indicator (what you measure), a Service Level Objective (your internal target), and a Service Level Agreement (your legal contract with users)? Why should engineers focus on SLOs?
3. **Metrics vs. Logs (Cardinality):** Why is it an anti-pattern to log highly variable data (like `user_id` or `request_uuid`) as Prometheus metrics? Explain "cardinality explosion" and how it crashes monitoring systems.
4. **Pull vs. Push Monitoring:** Why does Prometheus "scrape" (pull) metrics from applications instead of applications pushing them? How do you monitor short-lived batch jobs (like Laravel Artisan commands) that push metrics instead?
5. **The Four Golden Signals:** Google defines the four golden signals as Latency, Traffic, Errors, and Saturation. How do you measure each of these specifically for a Laravel application?
6. **Alert Fatigue & Actionability:** Why is alerting on "CPU > 80%" usually a bad idea? How do you design symptom-based alerts (e.g., "Checkout latency > 2s") vs. cause-based alerts to prevent engineers from ignoring notifications?
7. **Distributed Tracing & Context Propagation:** How does a single user request get tracked across PHP-FPM, Redis, MySQL, and background queues? Explain the W3C Trace Context standard and how trace IDs are passed via headers.
8. **Structured Logging:** Why is plain text logging (`Log::info('User logged in')`) insufficient for production? Explain JSON structured logging, mandatory fields (timestamp, level, correlation_id), and how they enable log aggregation.
9. **Log Aggregation & Cost Management:** Why is shipping every log to a central system (ELK/Datadog) expensive and unsustainable? How do you implement sampling, log levels, and retention policies to balance visibility with cost?
10. **Observability in Laravel:** How do tools like Laravel Telescope, Laravel APM, and OpenTelemetry PHP differ? When should you use framework-specific tools vs. vendor-neutral OpenTelemetry standards?

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Prometheus & Grafana Stack**<br>Deploy Prometheus and Grafana using Docker Compose or Kubernetes. Configure Prometheus to scrape a basic target (like Node Exporter or your Laravel app). Verify data appears in Grafana. | Prometheus scrapes successfully. Grafana dashboards load data. No authentication errors. |
| 2 | **Export Laravel Metrics**<br>Install a package (e.g., `lorisleiva/laravel-metrics` or `spatie/laravel-stats`) or build a `/metrics` endpoint. Expose custom metrics: `queue_depth`, `cache_hit_ratio`, and `active_sessions`. | Prometheus scrapes `/metrics` and shows custom Laravel metrics. |
| 3 | **Structured JSON Logging**<br>Configure Laravel's Monolog driver to output JSON logs. Ensure every log includes `timestamp`, `level`, `message`, `context`, and a custom `correlation_id`. | Logs are valid JSON. `correlation_id` is present. Can be parsed by standard JSON tools. |
| 4 | **Centralized Logging with Loki**<br>Deploy Grafana Loki. Configure Laravel to push JSON logs to Loki (via Promtail or direct HTTP). Query logs in Grafana using LogQL (e.g., `{app="multimart"} |= "error"`). | Logs flow to Loki. Grafana shows log streams. Queries filter by severity and context. |
| 5 | **Distributed Tracing (OpenTelemetry)**<br>Install the OpenTelemetry PHP SDK. Configure your Laravel app to export traces to Jaeger or Zipkin. Trigger a web request that dispatches a queue job and verify the full trace. | Jaeger shows a complete trace spanning HTTP request and Queue job. Spans are linked by trace ID. |
| 6 | **Define SLOs for MultiMart**<br>Write a document defining SLIs and SLOs for two key user journeys: "Add to Cart" and "Checkout". Define availability (e.g., 99.9%) and latency (e.g., p95 < 500ms). | Document clearly defines metrics, targets, and measurement windows. Team can agree on these targets. |
| 7 | **Alerting Rules in Prometheus**<br>Create an alerting rule in Prometheus for "High Error Rate" (HTTP 500s > 5% over 5m) and "Queue Depth Too High" (> 1000 jobs). Route alerts to a mock webhook (Discord/Slack). | Alerts fire correctly when thresholds are breached. Webhook receives the payload. Alerts resolve when metrics normalize. |
| 8 | **Golden Signals Dashboard**<br>Build a Grafana dashboard showing the Four Golden Signals for MultiMart. Include panels for request rate, p95 latency, error rate, and server CPU/Memory usage. | Dashboard is visually clear. Panels use correct Prometheus queries. Provides instant health overview. |
| 9 | **Pushgateway for Artisan Commands**<br>Configure a scheduled Artisan command (e.g., `php artisan multimart:sync`) to push its execution time and success/failure status to a Prometheus Pushgateway. | Pushgateway receives metrics. Prometheus scrapes Pushgateway. Command metrics are visible in Grafana. |
| 10| **Incident Simulation & Debugging**<br>Simulate a slow database query in MultiMart. Use your traces and metrics to find the exact bottleneck. Document the step-by-step debugging process using your observability stack. | Bottleneck identified within 5 minutes using traces. Debugging steps documented. Proves the stack actually helps. |
