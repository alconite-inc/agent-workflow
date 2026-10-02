---
name: kafka-best-practices
description: Design or review Kafka event contracts, consumer acknowledgment, retries, dead letters, and delivery guarantees. Use for Kafka producers and workers; combine with Spring Boot guidance for application wiring.
---

# Kafka best practices

Read the generated root [AGENTS.md](../../../AGENTS.md) for architecture,
pinned versions, worker configuration, and checks. Apply this technology guidance
with the assigned generic role; it neither launches agents nor expands write ownership.
Inspect the configured listener, producer, and error handler before changing delivery.

## Delivery and failure boundaries

- Preserve the configured manual acknowledgment flow. Acknowledge only after
  the intended processing outcome is durable, and review the difference between
  listener acknowledgment, offset commit, and business-side effects. Confirm
  listener-thread requirements before introducing asynchronous processing.
  See [listener acknowledgment modes](https://docs.spring.io/spring-kafka/reference/kafka/receiving-messages/message-listener-container.html).
- Expect replay across failure and rebalance boundaries. Producer idempotence
  does not deduplicate consumer business effects; Kafka transactions can join
  Kafka output and consumed offsets, but cannot alone make an external database
  or HTTP side effect exactly once. Use stable event IDs and an explicit
  deduplication/transaction strategy where required. See [delivery semantics](https://kafka.apache.org/41/design/design/#message-delivery-semantics).
- Choose keys and partitions from ordering requirements. Review ordering when
  changing partition counts, listener concurrency, retries, or asynchronous
  work; do not assume a global order across partitions.
- Keep retry attempts bounded and distinguish retryable failures from malformed
  events. Verify dead-letter publication failures are observable and cannot
  silently discard a record; document replay behavior and preserve identifiers.
  See [error handling and dead-letter recovery](https://docs.spring.io/spring-kafka/reference/kafka/annotation-error-handling.html).
- Keep payloads typed and schema changes explicit. Preserve narrow deserializer
  trust settings; do not enable arbitrary class/package deserialization to fix
  a type mismatch. Log identifiers and outcomes rather than complete payloads.

## Verification focus

Use the existing broker-backed tests for successful acknowledgment, retries,
dead letters, and poison messages. Add replay/idempotency checks when side effects
change and use isolated topics/groups with bounded waits. Mocked producer calls
do not prove broker delivery; an embedded broker does not establish production
replication, failover, or exactly-once guarantees. Run the root's worker checks.
