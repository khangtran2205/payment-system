# Microservices payments system in Go

What included:
- A double-entry ledger in PostgreSQL, type-safe SQL with sqlc and pgx, moving money safely with locks and idempotency keys
- Auth with JWTs, gRPC between services, an API gateway, events with NATS JetStream and the transactional outbox
- Transfers as sagas, notifications with a dead-letter queue, observability with OpenTelemetry, Prometheus and Grafana
- Testing with testcontainers and k6, distroless images and CI, signed webhooks, multi-currency FX, fraud checks, reconciliation
- Security with mTLS and least-privilege roles, performance tuning, Kubernetes with Helm, and chaos engineering that proves no money is ever lost

## The system

| Service | Port | Description |
|---|---|---|
| gateway | 8080 (internal 9080) | the only public entry point: JWT checks, rate limits, routing |
| wallet | 8081, gRPC 9081 | the double-entry ledger, holds, idempotency, FX conversion |
| auth | 8082 | users, argon2id passwords, EdDSA access tokens, rotating refresh tokens |
| transfer | 8083 | transfers as sagas: hold, capture or release, with retries and compensation |
| notification | 8084 | emails from events, idempotent consumers, a dead-letter queue |
| webhook | 8085 | signed webhook deliveries with retries, a delivery log and replay |