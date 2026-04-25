# Circuit Breaker (circuit-breaker)

The Circuit Breaker is a stability pattern for distributed systems and API architectures that prevents cascading failure when a downstream service degrades. A breaker wraps a remote call and tracks failures against a threshold; when the threshold is exceeded the breaker "opens" and short-circuits subsequent calls (typically returning an error or fallback) without contacting the downstream service. After a cooldown the breaker enters a "half-open" probe state and either resets to "closed" on success or re-opens on failure. The pattern was popularized by Michael Nygard in *Release It!* and is now standard in resilient microservice and API gateway design.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/circuit-breaker/refs/heads/main/apis.yml)

## Scope

- **Type:** Topic
- **Position:** Reference
- **Access:** 3rd-Party

## Tags

- Circuit Breaker
- Distributed Systems
- Fault Tolerance
- Microservices
- Patterns
- Resilience
- Stability

## Timestamps

- **Created:** 2025-01-01
- **Modified:** 2026-04-23

## States

A circuit breaker is a small state machine over three primary states:

- **CLOSED** — calls flow through normally; failures are counted in a sliding window.
- **OPEN** — calls are short-circuited (rejected immediately or routed to a fallback) without touching the protected resource.
- **HALF_OPEN** — after a cooldown, a limited number of probe calls are permitted; their outcome decides whether the breaker resets to CLOSED or returns to OPEN.

Most production implementations add `DISABLED` and `FORCED_OPEN` for operator overrides.

## Why use it

- **Fail fast** — stop holding threads, sockets, and connection pool slots open against a service that is already down.
- **Protect downstream services** — let an overloaded service recover without continued load.
- **Provide a fallback path** — return cached data, default values, or a degraded response instead of an error.
- **Surface health** — breaker state is a first-class signal in dashboards, alerting, and SLO calculation.

## Key Implementations

| Name | Language / Layer | Notes |
|---|---|---|
| [Resilience4j](https://resilience4j.readme.io/docs/circuitbreaker) | Java | Modern, functional successor to Hystrix |
| [Hystrix](https://github.com/Netflix/Hystrix) | Java | Netflix's original; in maintenance mode since 2018 |
| [Polly](https://www.pollydocs.org/strategies/circuit-breaker.html) | .NET | Classic and rate-of-failure breaker strategies |
| [opossum](https://nodeshift.dev/opossum/) | Node.js | Promise-based with Hystrix-compatible metrics |
| [gobreaker](https://github.com/sony/gobreaker) | Go | Sony's idiomatic Go implementation |
| [Hystrix-Go](https://github.com/afex/hystrix-go) | Go | Go port modelled on Netflix Hystrix |
| [pybreaker](https://github.com/danielfm/pybreaker) | Python | With optional Redis-backed shared state |
| [Failsafe](https://failsafe.dev/circuit-breaker/) | Java | Combined breaker, retry, timeout, fallback, rate-limiter |
| [Istio Outlier Detection](https://istio.io/latest/docs/reference/config/networking/destination-rule/#OutlierDetection) | Service Mesh | Ejects unhealthy hosts at the mesh layer |
| [Envoy Circuit Breaking](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/upstream/circuit_breaking) | Service Mesh | Concurrency-based: max connections, pending, retries |
| [Spring Cloud Circuit Breaker](https://docs.spring.io/spring-cloud-circuitbreaker/docs/current/reference/html/) | Framework | Abstraction over Resilience4j, Sentinel, etc. |

## References

- [Martin Fowler — *CircuitBreaker*](https://martinfowler.com/bliki/CircuitBreaker.html)
- [Microsoft Azure Architecture Center — *Circuit Breaker pattern*](https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker)
- [Michael Nygard — *Release It!*](https://pragprog.com/titles/mnee2/release-it-second-edition/)

## Vocabulary

- [JSON-LD Context](json-ld/circuit-breaker-context.jsonld) — covers `CircuitBreaker`, `State`, `StateTransition`, `Metrics`, and `Fallback` types.
- [State JSON Schema](json-schema/circuit-breaker-state-schema.json) — runtime state suitable for status endpoints and dashboards.
- [Configuration JSON Schema](json-schema/circuit-breaker-config-schema.json) — common configuration fields distilled from major implementations.

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
