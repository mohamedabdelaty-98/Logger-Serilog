# 11. Production Logging Best Practices

## Make events actionable

- Record meaningful outcomes, failures, retries, and important state transitions.
- Include stable identifiers and operation context needed for investigation.
- Use correct severity; expected business decisions are not automatically errors.
- Pass exception objects separately and add contextual properties.
- Avoid logging the same failure repeatedly at every layer.
- Prefer structured templates and consistent property names.

## Protect data

Never log passwords, access tokens, API keys, connection strings, payment card data, or secrets. Apply the organization's privacy rules to personal data, request bodies, headers, and identifiers. Log only the minimum fields needed for diagnosis. Sanitize untrusted values where log injection could occur, and restrict log access and retention like other sensitive operational data.

## Control volume and cost

Use levels and category overrides intentionally. Avoid per-item success logs in high-volume loops unless they answer a real operational need. Prefer aggregate summaries or metrics for counts and rates. Set sink retention, size limits, and ingestion budgets; monitor for sudden volume growth.

## Correlate across a request

Use trace context and stable correlation identifiers across service boundaries. Include `TraceId` and `SpanId` through tracing instrumentation where available. Do not use ThreadId as request correlation. For background jobs, include job identity and attempt because there may be no HTTP request context.

## Handle failures and latency honestly

Record retries with attempt and delay; distinguish transient failure from final failure. Include elapsed time when it helps identify slow external calls. Do not claim an operation succeeded before its durable transaction or external outcome is known. Keep logging failure handling from masking the primary application exception.

## Performance and reliability

- Avoid expensive object construction when a level is disabled; use source-generated logging in hot paths when warranted.
- Keep templates static and avoid interpolation.
- Choose sinks with known buffering, retry, shutdown, and outage behavior.
- Flush buffered events during orderly shutdown.
- Prefer host-managed console collection in containers unless a durable file requirement exists.
- Keep thread identifiers supplementary; asynchronous work changes threads.

## Logging is not metrics or tracing

Use metrics for numeric aggregates and alert thresholds; use traces for causal timing across services; use logs for detailed event facts. Link them with trace identifiers when possible. A large number of log lines is not a substitute for useful telemetry design.
