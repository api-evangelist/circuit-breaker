# Circuit Breaker (circuit-breaker)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
