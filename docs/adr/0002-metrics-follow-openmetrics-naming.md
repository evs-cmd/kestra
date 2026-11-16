---
status: accepted
date: 2026-11-16
---

# 2. Metrics follow OpenMetrics naming, and every Counter ends in `.total`

## Context

Kestra exposes its metrics to Prometheus through Micrometer. `MetricRegistry.java` had collected inconsistent names:
manual `.count` suffixes, Counters with no `.total`, `.total` on Gauges, mixed dots and underscores, two metrics with
one description, and Java constants whose names implied another type. Each made dashboards and alerts harder to
write and names harder to grep.

Source: [Metrics Guidelines](../architecture/METRICS_GUIDELINES.md).

## Decision

Metrics use the OpenMetrics 1.0 exposition format, written through Micrometer's `MeterRegistry`, and follow these
names:

- Every Counter ends in `.total`, with no exception, and nothing else does.
- Names are dot notation, lowercase, ordered `<system>.<subject>.<qualifier>`.
- Gauges take no `.count` or `.total` suffix. Timers end in `.duration` or a unit. Units are base units (seconds,
  bytes, ratios from 0 to 1).
- Labels have bounded cardinality: no execution IDs, full URLs or free-form input.
- Every meter has a unique description, and the Java constant mirrors the metric's name and type.

Renaming or retyping a metric is a breaking change: the new metric ships beside the old, deprecated one for one
release, and the old one goes in the next.

## Consequences

- The Micrometer name, the Java constant and the exposed Prometheus name line up one to one, so a name can be
  grepped across code and dashboards.
- Related metrics sort together, by system and then subject.
- Fixing an existing bad name costs a deprecation window, unless no dashboard or alert can depend on it yet.
- Reviews of a new metric have a checklist to apply, in the source page.
