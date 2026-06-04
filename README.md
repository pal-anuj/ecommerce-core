# ecommerce-core
A production-grade microservices e-commerce backend demonstrating distributed system patterns — built with Spring Boot 3.x, Java 21, Kafka, Redis, and PostgreSQL.

![Status](https://img.shields.io/badge/status-in--progress-yellow)
![Java](https://img.shields.io/badge/Java-21-orange)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x-brightgreen)
![License](https://img.shields.io/badge/license-MIT-blue)

---

## 📌 Overview

`commerce-core` is a distributed e-commerce backend that simulates real-world order management at scale. It's built as a learning + portfolio project to demonstrate hands-on expertise with the patterns and technologies expected at the Senior Backend Engineer level.

The system is composed of **6 independently deployable microservices** that communicate through a mix of synchronous REST calls (via an API Gateway) and asynchronous Kafka events. Failures are handled through the **Saga orchestration pattern** with compensating transactions, and resilience is built in via **circuit breakers, retries, and bulkheads**.

> 🚧 **Status:** Active development. Built incrementally as part of a structured 12-week Senior Backend Engineer preparation roadmap.

---

## 🏗️ Architecture

```
                          ┌──────────────────┐
                          │   API Gateway    │
                          │  (Spring Cloud)  │
                          └────────┬─────────┘
                                   │
        ┌──────────────────────────┼──────────────────────────┐
        │                          │                          │
        ▼                          ▼                          ▼
┌──────────────┐         ┌──────────────────┐       ┌────────────────┐
│ User Service │         │ Product Service  │       │ Order Service  │
│  JWT Auth    │         │  Catalog + Cache │◄──────┤  Saga Orch.    │
└──────┬───────┘         └────────┬─────────┘       └────────┬───────┘
       │                          │                          │
       ▼                          ▼                          ▼
   PostgreSQL                PostgreSQL                  PostgreSQL
                              + Redis                        +
                                                          ┌──┴───┐
                                                          │ Kafka│
                                                          └──┬───┘
                                            ┌────────────────┼───────────────┐
                                            ▼                                ▼
                                  ┌──────────────────┐           ┌──────────────────────┐
                                  │ Payment Service  │           │ Notification Service │
                                  │   PostgreSQL     │           │   Kafka Consumer     │
                                  └──────────────────┘           └──────────────────────┘
```

---

## 🧩 Services

| Service | Responsibility | Key Tech |
|---------|----------------|----------|
| **API Gateway** | Routing, rate limiting, auth forwarding, request tracing | Spring Cloud Gateway, Resilience4j |
| **User Service** | Registration, login, JWT issuance, role-based access | Spring Security, JJWT, PostgreSQL |
| **Product Service** | Product CRUD, search, inventory, distributed caching | Spring Data JPA, Redis, PostgreSQL |
| **Order Service** | Order lifecycle, Saga orchestration, state machine | Spring Boot, Kafka, PostgreSQL |
| **Payment Service** | Payment processing (mock), refunds, idempotency | Spring Boot, PostgreSQL |
| **Notification Service** | Email/SMS dispatch on order events | Spring Boot, Kafka Consumer |

---

## 🛠️ Tech Stack

**Core**
- Java 21
- Spring Boot 3.x (Web, Data JPA, Security, Cloud)
- Maven

**Data**
- PostgreSQL 16 (primary store, one DB per service)
- Redis 7 (product catalog cache, rate limit counters)
- Flyway (schema migrations)

**Messaging**
- Apache Kafka (event-driven communication)

**Resilience & Observability**
- Resilience4j (circuit breaker, retry, bulkhead, rate limiter)
- Spring Boot Actuator + Micrometer
- Prometheus + Grafana (metrics)
- Zipkin (distributed tracing)
- SLF4J + Logback with MDC correlation IDs

**Testing**
- JUnit 5, Mockito, AssertJ
- TestContainers (real DB in integration tests)
- MockMvc, WireMock

**DevOps**
- Docker (multi-stage builds)
- docker-compose (local full-stack)
- Jenkins (CI pipeline)
- Kubernetes manifests (deployment target)

---

## ✨ Key Patterns Demonstrated

- ✅ **API Gateway pattern** — single entry point with cross-cutting concerns
- ✅ **Saga orchestration** — distributed transaction with compensating actions
- ✅ **Circuit Breaker** — graceful degradation when downstream services fail
- ✅ **Event-driven architecture** — Kafka topics for order, payment, inventory events
- ✅ **Cache-aside pattern** — Redis caching with TTL and invalidation strategy
- ✅ **Idempotency** — safe retry semantics for payment operations
- ✅ **JWT auth with refresh tokens** — stateless authentication across services
- ✅ **Rate limiting** — token bucket algorithm via Redis
- ✅ **Distributed tracing** — correlation IDs propagated across all services

---

## 🚀 Getting Started

> 📌 Setup instructions will be added as services come online (currently scaffolding).

**Prerequisites (planned):**
```bash
- Java 21
- Docker & Docker Compose
- Maven 3.9+
```

**Run locally (planned):**
```bash
git clone https://github.com/pal-anuj/commerce-core.git
cd commerce-core
docker-compose up
```

---

## 📊 Roadmap

- [x] Repository setup + architecture plan
- [ ] **Week 5** — User Service (JWT auth, roles)
- [ ] **Week 5–6** — Product Service (CRUD + Redis cache)
- [ ] **Week 7** — Order Service + API Gateway + Feign clients
- [ ] **Week 7** — Circuit breakers (Resilience4j)
- [ ] **Week 8** — Payment + Notification Service
- [ ] **Week 8** — Saga orchestration for end-to-end order flow
- [ ] **Week 9** — Redis caching + rate limiting + Actuator metrics
- [ ] **Week 10** — Dockerize all services + docker-compose
- [ ] **Week 10** — Structured JSON logging + Zipkin tracing
- [ ] **Week 11** — Jenkinsfile + Kubernetes manifests
- [ ] **Week 12** — Final documentation + architecture deep-dive

---

## 📐 Design Decisions

This section will document the **why** behind key technical choices — the kind of discussion you'd have in a system design interview.

> Detailed write-ups added as features are built.

Topics to be covered:
- Why Saga orchestration over choreography for this domain
- PostgreSQL vs DynamoDB for order data — consistency tradeoff
- Why Kafka over RabbitMQ for event communication
- Cache invalidation strategy: TTL vs event-driven invalidation
- Circuit breaker thresholds — how we picked them

---

## 👤 Author

**Anuj Kumar Pal**  
Senior Software Engineer | Java Backend & Distributed Systems  
[LinkedIn](https://www.linkedin.com/in/pal-anuj/) · [GitHub](https://github.com/pal-anuj)

---

## 📄 License

MIT License — see [LICENSE](./LICENSE) for details.
