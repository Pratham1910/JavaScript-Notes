# The Complete System Design Tutorial — From Scaling Basics to Real Architectures

*System design isn't a fixed syllabus — it's a way of reasoning about trade-offs under constraints (traffic, data size, consistency needs, cost) that don't fit on one machine. This guide builds that reasoning from the ground up: scaling → networking → caching → data → consistency → reliability, then applies all of it to real interview questions and real companies' documented architectures.*

---

## Table of Contents

**Part I — Foundations**

1. [What system design actually tests](#what-system-design-actually-tests)
2. [Vertical vs. horizontal scaling](#vertical-vs-horizontal-scaling)
3. [Latency vs. throughput](#latency-vs-throughput)
4. [Back-of-the-envelope estimation](#back-of-the-envelope-estimation)

**Part II — Networking Layer**
5. [DNS and how a request finds a server](#dns-and-how-a-request-finds-a-server)
6. [Load balancers](#load-balancers)
7. [CDNs](#cdns)
8. [Proxy vs. reverse proxy](#proxy-vs-reverse-proxy)

**Part III — Scaling the Application Layer**
9. [Stateless vs. stateful services](#stateless-vs-stateful-services)
10. [Caching](#caching)

**Part IV — The Data Layer**
11. [SQL vs. NoSQL](#sql-vs-nosql)
12. [Indexing](#indexing)
13. [Replication](#replication)
14. [Sharding (partitioning)](#sharding-partitioning)
15. [CAP theorem and PACELC](#cap-theorem-and-pacelc)
16. [ACID vs. BASE](#acid-vs-base)

**Part V — Distributed Systems Concepts**
17. [Consistency models](#consistency-models)
18. [Consensus: Paxos and Raft, at the level you need](#consensus-paxos-and-raft-at-the-level-you-need)
19. [Distributed locking](#distributed-locking)
20. [Message queues, recap](#message-queues-recap)

**Part VI — Reliability & Performance Patterns**
21. [Rate limiting algorithms](#rate-limiting-algorithms)
22. [Health checks and failover](#health-checks-and-failover)
23. [Idempotency](#idempotency)

**Part VII — Storage & Search**
24. [Blob storage](#blob-storage)
25. [Search: inverted indexes](#search-inverted-indexes)

**Part VIII — Designing Real Systems**
26. [Design a URL shortener](#design-a-url-shortener)
27. [Design a rate limiter](#design-a-rate-limiter)
28. [Design a news feed (Twitter/Instagram)](#design-a-news-feed-twitterinstagram)
29. [Design a chat system (WhatsApp)](#design-a-chat-system-whatsapp)
30. [Design a distributed cache](#design-a-distributed-cache)

**Part IX — Real Architectures**
31. [Twitter: fan-out and the timeline problem](#twitter-fan-out-and-the-timeline-problem)
32. [Dropbox: how file sync actually works](#dropbox-how-file-sync-actually-works)
33. [Netflix: CDN + microservices at global scale](#netflix-cdn--microservices-at-global-scale)

**Part X — Roadmap & Practice**
34. [A learning roadmap](#a-learning-roadmap)
35. [Question bank](#question-bank)
36. [Projects to build, in order](#projects-to-build-in-order)

---

## Part I — Foundations

### What system design actually tests

System design has no single correct answer, and that's the entire point — it tests **how you navigate trade-offs under constraints**, not whether you know one "right" architecture. Every decision in this guide trades something for something else: more caching means faster reads but staler data; more replicas mean more availability but harder consistency; sharding means more write capacity but no more cross-shard transactions. An interviewer (or a real architecture review) is evaluating whether you can name the trade-off explicitly and justify the side you picked for *this* system's actual requirements — not whether you memorized "the Twitter architecture."

**The four questions worth asking before designing anything**, in order:

1. **What does this system actually need to do?** (functional requirements — read-heavy or write-heavy? real-time or can it be eventually consistent?)
2. **At what scale?** (Part I.4 — how many users, how much data, how much traffic — a design for 1,000 users and one for 100 million users are different systems, not the same system "done bigger")
3. **What can it sacrifice?** (Part IV.5's CAP theorem — perfect consistency, perfect availability, and perfect partition tolerance can't all be had at once; which does this system's use case actually require?)
4. **What's the cost budget?** (a solution that's technically superior but costs 50x more than the business can justify is a wrong answer in practice, even if it's a right answer in theory)

> **Why this matters before any specific pattern.** Every pattern in Parts II–VII below is a tool, not a mandate — a system with 500 daily users does not need sharding, multi-region replication, or a message queue, and reaching for them anyway is the same premature-complexity mistake the Microservices guide's Part IX calls out for services. Match the tool to the actual scale and actual requirement, every time.

### Vertical vs. horizontal scaling

When a single server can no longer handle the load, there are exactly two directions to go:

|                            | Vertical scaling (scale up)                                                                            | Horizontal scaling (scale out)                                                                                                             |
| -------------------------- | ------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **What it means**    | Add more CPU/RAM/disk to the*same* machine                                                           | Add*more machines*, splitting load across them                                                                                           |
| **Ceiling**          | Hard — there's a maximum machine size money can buy                                                   | Effectively unlimited — keep adding machines                                                                                              |
| **Complexity added** | Minimal — the application code doesn't need to change at all                                          | Real — now you need load balancing (Part II.2), and if the service holds any state, that state must be shared or partitioned (Part III.1) |
| **Failure impact**   | A single point of failure — if that one (bigger) machine dies, everything is down                     | One machine dying takes out only its share of capacity — the rest keep serving                                                            |
| **Cost curve**       | Cost rises*faster* than capacity near the top end (the biggest machines carry a steep price premium) | Roughly linear cost per unit of capacity, using many commodity-sized machines                                                              |

```
Vertical:                          Horizontal:

┌─────────────┐                    ┌───────┐ ┌───────┐ ┌───────┐
│             │                    │Server │ │Server │ │Server │
│   Bigger    │   4 CPU → 32 CPU   │   A   │ │   B   │ │   C   │
│   Server    │  ───────────────▶  └───┬───┘ └───┬───┘ └───┬───┘
│             │                        └─────────┼─────────┘
└─────────────┘                           Load Balancer
```

> **Why almost every large-scale system ends up horizontal eventually.** Vertical scaling is the right *first* move — it's simpler, and for a lot of real systems (especially early-stage products) a bigger single database instance solves the problem for years with zero architectural complexity added. But it has a hard ceiling (the largest cloud instance available), and it never solves the single-point-of-failure problem — the biggest machine in the world still goes down sometimes. Horizontal scaling is what's required once traffic exceeds any single machine's ceiling, or once "one machine going down shouldn't take the whole system down" becomes a real requirement (which, for a lot of production systems, is true from day one regardless of raw scale).

> **Mistake.** Reaching for horizontal scaling (and the load-balancing, statelessness, and data-partitioning complexity that comes with it) before actually hitting vertical scaling's ceiling. A surprising fraction of real systems never need to scale horizontally at all — checking "have we actually maxed out a single, reasonably-sized machine" before adding distributed-systems complexity is a cheap, high-value sanity check.

### Latency vs. throughput

Two different measurements of performance that are easy to conflate and often trade off against each other:

- **Latency** — how long a *single* request takes, start to finish (measured in ms).
- **Throughput** — how many requests the system can process *in total*, per unit time (measured in requests/sec).

> **Why they aren't the same thing, concretely.** A system can have low latency but low throughput (a single very fast worker, but only one of them — great for the one request being served, bad if 10,000 arrive at once) or high throughput but higher latency (batching 1,000 requests together before processing improves total throughput, but each individual request now waits for the batch to fill). Batching, queueing, and connection pooling are all classic throughput-for-latency trades — worth naming explicitly whenever a design decision touches either.

**Percentiles matter more than averages.** A system's *average* latency can look fine while a meaningful fraction of real users have a terrible experience — this is why production systems report p50/p95/p99 (the latency below which 50%/95%/99% of requests complete), not just a mean:

```
p50 (median):  45ms   — half of all requests are at least this fast
p95:          180ms   — 95% of requests are at least this fast
p99:          850ms   — the slowest 1% — often reveals a real, fixable issue
                          (a specific slow query path, GC pauses, a cold cache)
```

> **Why p99 gets specific design attention.** At the scale of a large system (millions of requests/day), "only" 1% of requests being slow is still tens of thousands of genuinely bad user experiences per day — and the p99 tail is frequently caused by one identifiable, fixable issue (as above), which is exactly why real systems design and monitor against tail latency, not just the average.

### Back-of-the-envelope estimation

Before designing anything, a rough estimate of scale determines which patterns in this guide are even relevant — the numbers below are the ones worth memorizing because they recur in nearly every estimation:

| Quantity                            | Rough value                          |
| ----------------------------------- | ------------------------------------ |
| 1 KB                                | ~1,000 bytes                         |
| 1 MB                                | ~1,000 KB (10⁶ bytes)               |
| 1 GB                                | ~1,000 MB (10⁹ bytes)               |
| Seconds in a day                    | ~86,400 (~100,000 for easy rounding) |
| L1 cache reference                  | ~1 ns                                |
| Main memory (RAM) reference         | ~100 ns                              |
| SSD random read                     | ~100 μs (1,000x slower than RAM)    |
| Round trip within same datacenter   | ~0.5 ms                              |
| Round trip, cross-continent         | ~150 ms                              |
| Reading 1 MB sequentially from disk | ~1 ms (SSD)                          |

**A worked example — "design a system for 10 million daily active users, each posting 2 times/day, each post averaging 1 KB":**

```
Writes/day  = 10,000,000 users × 2 posts = 20,000,000 posts/day
Writes/sec  = 20,000,000 / 86,400 ≈ 231 writes/sec (average)
Peak writes/sec ≈ 231 × 3 (typical peak-to-average ratio) ≈ 700/sec

Storage/day = 20,000,000 posts × 1 KB ≈ 20 GB/day
Storage/year ≈ 20 GB × 365 ≈ 7.3 TB/year
```

> **Why this exercise matters before picking any technology.** 700 writes/sec is comfortably within a single well-tuned relational database's capacity — no sharding (Part IV.4) needed yet. 7.3 TB/year is real but entirely manageable storage, not a "we need a custom distributed filesystem" scale. Running these numbers *first* prevents both under-designing (a single unindexed table that falls over at real load) and over-designing (building a sharded, multi-region system for a workload one database could handle for years).

---

## Part II — Networking Layer

### DNS and how a request finds a server

Before any request reaches your system, a domain name (`api.example.com`) must resolve to an IP address — DNS is the distributed, hierarchical lookup system that does this, and it's usually the very first hop in any request's path, cached aggressively at multiple layers (browser, OS, ISP) precisely because it would otherwise add a lookup to every single request.

```
Browser → checks local cache → checks OS cache → asks a DNS Resolver
        → Resolver asks a Root server → asks a TLD server (.com)
        → asks example.com's Authoritative server → returns the IP
        → Browser connects to that IP directly for the actual request
```

> **Why this matters for system design specifically.** DNS is also a (coarse) load-balancing and failover tool: a domain can resolve to multiple IPs (round-robin DNS) or be configured to fail over to a backup region's IP if health checks against the primary fail — cheap, simple, but coarse-grained compared to the load balancers below, since DNS resolutions are cached client-side for a TTL and can't react to real-time load.

### Load balancers

A load balancer sits in front of a pool of servers and distributes incoming requests across them — the single most common building block for horizontal scaling (Part I.2).

**Layer 4 vs. Layer 7 — the distinction that matters for what the balancer can actually do:**

|              | Layer 4 (transport)                               | Layer 7 (application)                                                                           |
| ------------ | ------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| Operates on  | Raw TCP/UDP — IP address and port only           | Actual HTTP content — URL path, headers, cookies                                               |
| Can route by | Nothing content-aware — just spreads connections | Path (`/api/*` → service A, `/static/*` → service B), headers, cookies (session affinity) |
| Speed        | Faster — no need to parse the request            | Slightly slower — must terminate and inspect the request                                       |
| Use case     | Raw throughput, non-HTTP protocols                | Anything needing content-aware routing — most modern web APIs                                  |

**Common algorithms for choosing which backend server gets the next request:**

```
Round Robin:        server 1, server 2, server 3, server 1, server 2, ...
                     (simple, assumes all servers and all requests are equal)

Least Connections:   send to whichever server currently has the fewest
                      active connections (better when request cost varies a lot)

Weighted Round Robin: like round robin, but bigger servers get proportionally
                       more requests (useful with a heterogeneous fleet)

Consistent Hashing:   hash(request key) → server (see Part IV.4 — critical
                       when you need the SAME client/key to keep landing on
                       the same server, e.g. for cache locality)
```

> **Why "least connections" often beats round robin in practice.** Round robin assumes every request costs the same amount of work — false the instant request costs vary (one user's request triggers a 2-second report generation, another's is a 5ms cache hit). Least-connections naturally routes new requests away from a server that's currently bogged down with slow requests, without needing to know in advance which requests are expensive.

**The load balancer itself is a single point of failure unless it's redundant** — production setups run at least two load balancer instances (often behind a floating/virtual IP, or DNS-based failover) specifically so the load balancer doesn't recreate the exact single-point-of-failure problem horizontal scaling was meant to solve in the first place.

### CDNs

A **Content Delivery Network** caches content (images, videos, JS/CSS bundles, and increasingly API responses) at edge locations physically close to end users, so a user in Mumbai gets a cached copy from a nearby edge server instead of a round trip to an origin server in Virginia.

```
User (Mumbai) → nearest CDN edge node (Mumbai) → CACHE HIT: served in ~10ms
                       │
                       └─ CACHE MISS: edge node fetches from Origin Server
                          (Virginia, ~250ms), caches it, THEN serves the user
                          (subsequent users in Mumbai now get the fast path)
```

> **Why this matters beyond "it's faster."** A CDN also directly reduces load on the origin server (many requests never reach it at all) and improves resilience (a brief origin outage may be invisible to users still being served cached content). It's the highest-leverage, lowest-effort scaling technique available for read-heavy, cacheable content — reach for a CDN before reaching for any of the harder scaling techniques later in this guide, for anything that qualifies (static assets, and increasingly API responses that don't change per-user).

### Proxy vs. reverse proxy

- **Forward proxy** — sits in front of *clients*, making requests on their behalf (a corporate proxy hiding internal client IPs from the internet, or bypassing geo-restrictions).
- **Reverse proxy** — sits in front of *servers*, receiving client requests and forwarding them to the appropriate backend (hiding backend topology from clients — this is what a load balancer and API Gateway, from the Microservices guide's Part II, both fundamentally are).

```
Forward proxy:    Client → Proxy → Internet             (hides the CLIENT)
Reverse proxy:    Internet → Proxy → Backend servers      (hides the SERVER)
```

Common reverse-proxy software (Nginx, HAProxy, Envoy) frequently does double duty as a load balancer, TLS-termination point, and static-content cache all at once — the roles overlap in practice even though the concepts are distinct.

---

## Part III — Scaling the Application Layer

### Stateless vs. stateful services

A **stateless** service keeps no client-specific data in memory between requests — every request carries everything the service needs to process it, and any of the load balancer's backend servers can handle any request equally well. A **stateful** service holds session or connection data in a specific server's memory (a user's shopping cart, a WebSocket connection) — meaning that user's *subsequent* requests must reach that *same* server.

> **Why statelessness is the default goal for horizontal scaling.** A stateless service can be scaled by simply adding more identical instances behind a load balancer using any algorithm (Part II.2) — no request needs to reach any particular instance. A stateful service either needs **sticky sessions** (the load balancer routes a given client to the same backend every time, via a cookie or consistent hashing — Part IV.4) or needs to externalize its state into a shared store (Redis for session data, a database) so any instance can serve any request. Externalizing state is almost always the better long-term answer — it keeps the application layer stateless and puts the actual state-management problem in a system built for exactly that (Part IV).

```js
// Stateful (bad for horizontal scaling) — session lives in this process's memory;
// only THIS server instance knows this user is logged in
const sessions = new Map();
app.post('/login', (req, res) => {
  sessions.set(req.sessionId, { userId: user.id });
});

// Stateless — session lives in Redis, shared across every instance;
// ANY server behind the load balancer can serve this user's next request
app.post('/login', async (req, res) => {
  await redis.set(`session:${req.sessionId}`, JSON.stringify({ userId: user.id }));
});
```

### Caching

Caching is, after CDNs, the second-highest-leverage scaling technique — storing a computed or fetched result somewhere faster to access than recomputing/refetching it, at the cost of that result potentially going stale.

**Where caching happens, layer by layer — a request typically passes through several caches before reaching the actual database:**

```
Browser cache → CDN (Part II.3) → Application-level cache (Redis/Memcached)
              → Database query cache → Database itself
```

**Cache-aside (lazy loading)** — the most common pattern; the application checks the cache first, and on a miss, reads from the database and populates the cache for next time:

```js
async function getUser(userId) {
  const cached = await redis.get(`user:${userId}`);
  if (cached) return JSON.parse(cached);            // cache hit

  const user = await db.query('SELECT * FROM users WHERE id = ?', [userId]);
  await redis.set(`user:${userId}`, JSON.stringify(user), 'EX', 3600); // cache miss — populate, 1hr TTL
  return user;
}
```

**Write-through** — every write goes to the cache *and* the database together, synchronously, so the cache is never stale but every write pays the latency of both:

```js
async function updateUser(userId, data) {
  await db.query('UPDATE users SET ... WHERE id = ?', [userId]);
  await redis.set(`user:${userId}`, JSON.stringify(data), 'EX', 3600); // keep cache in sync immediately
}
```

**Write-back (write-behind)** — writes go to the cache immediately (fast) and are flushed to the database asynchronously in the background — fastest writes, but a cache failure before the flush means real, uncommitted data loss.

> **The trade-off table, made explicit:**

| Strategy      | Read speed after miss          | Write speed                                                      | Risk                                                   |
| ------------- | ------------------------------ | ---------------------------------------------------------------- | ------------------------------------------------------ |
| Cache-aside   | Slow on first miss, fast after | Fast (cache untouched on write, just invalidated/left to expire) | Stale cache until TTL expires or explicit invalidation |
| Write-through | Always fast                    | Slower (writes both cache and DB)                                | None on cache side — DB and cache always agree        |
| Write-back    | Always fast                    | Fastest                                                          | Data loss if the cache crashes before flushing to DB   |

**Cache invalidation — the hard part.** ("There are only two hard things in computer science: cache invalidation and naming things.") The core question is always: *when the underlying data changes, how does the cache find out?* Three common strategies:

- **TTL (time-to-live)** — the cache entry simply expires after a fixed duration, whether or not the underlying data actually changed. Simple, but there's always a window where the cache can serve stale data.
- **Explicit invalidation** — the write path itself deletes or updates the corresponding cache key the moment the data changes (the write-through pattern above). Correct immediately, but requires every code path that writes this data to remember to do it.
- **Event-driven invalidation** — a data-change event (see the Microservices guide's Part II async messaging) triggers cache invalidation across every service/instance that might be caching that data, useful when several independent services cache the same underlying data.

> **Mistake.** Caching without ever considering the invalidation strategy up front — "we'll just cache it" without deciding TTL vs. explicit invalidation leads to either serving badly stale data (TTL set too long) or a cache that provides almost no benefit (TTL set too short "to be safe"). Decide the acceptable staleness window as part of the requirements gathering (Part I.1's "what does this system actually need"), not as an afterthought.

**Cache eviction — what happens when the cache is full.** LRU (Least Recently Used — evict what hasn't been accessed in the longest time) is the default in most systems (Redis supports it natively) because it matches real access patterns well: recently-used data is disproportionately likely to be requested again soon.

---

## Part IV — The Data Layer

### SQL vs. NoSQL

|               | SQL (relational)                                                                                                      | NoSQL                                                                                                |
| ------------- | --------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| Schema        | Fixed, defined up front — enforced by the database                                                                   | Flexible/schemaless (document stores) or purpose-built (key-value, wide-column, graph)               |
| Relationships | First-class — joins across tables                                                                                    | Generally denormalized — related data often duplicated instead of joined                            |
| Consistency   | Strong, ACID transactions (Part IV.6) by default                                                                      | Varies widely — many default to eventual consistency for availability/scale                         |
| Scaling       | Traditionally vertical; horizontal (sharding) is possible but harder                                                  | Many are built for horizontal scaling from the ground up                                             |
| Best for      | Data with real relational structure and a need for multi-row transactional correctness (financial ledgers, inventory) | High-volume, simpler-access-pattern data (user sessions, event logs, product catalogs at huge scale) |
| Examples      | PostgreSQL, MySQL                                                                                                     | MongoDB (document), Redis (key-value), Cassandra (wide-column), Neo4j (graph)                        |

> **The decision isn't "SQL is old, NoSQL is scalable" — that's a common, wrong simplification.** Modern relational databases scale to enormous size with the right techniques (indexing, replication, sharding — all covered below), and plenty of huge-scale systems (banking, most e-commerce order systems) are built on relational databases specifically *because* they need real ACID transactions across related tables. Reach for NoSQL when the access pattern genuinely doesn't need relational joins/transactions (a key-value session store, a document store for loosely-structured content) or when the specific NoSQL system's scaling model (e.g., Cassandra's write-optimized, leaderless replication) matches the workload better than a relational database's model would.

### Indexing

An index is a separate data structure (almost always a B-tree, conceptually similar to the BST/balanced-tree structures in the DSA guide's Chapter 10) that lets the database find rows matching a condition without scanning every row — the single highest-impact, lowest-effort database performance technique available.

```sql
-- Without an index: the database scans every row in the table — O(n)
SELECT * FROM users WHERE email = 'alice@example.com';

-- With an index on `email`: the database does a B-tree lookup — O(log n)
CREATE INDEX idx_users_email ON users(email);
```

> **The trade-off indexing makes.** Every index speeds up reads that filter/sort on that column, but slows down writes (every `INSERT`/`UPDATE` must also update the index's B-tree) and consumes additional storage. The mistake in both directions is common: no indexes at all on a table with millions of rows makes every query a full scan; indexing *every* column "just in case" on a write-heavy table can measurably slow down writes for indexes that rarely get used by an actual query. Index the columns that actually appear in `WHERE`, `JOIN`, and `ORDER BY` clauses of real, frequent queries — check with the database's own query planner (`EXPLAIN` in PostgreSQL/MySQL), not by guessing.

### Replication

Keeping copies of the same data on multiple database servers — for read scaling, and for availability if one server fails.

**Leader-follower (primary-replica) replication** — the most common setup: all writes go to one leader, which asynchronously (or synchronously) propagates changes to one or more followers; reads can be served from any follower, spreading read load across multiple machines:

```
                    Writes
                       │
                       ▼
                 ┌──────────┐
                 │  Leader  │
                 └────┬─────┘
        replicates    │    replicates
        ┌─────────────┼─────────────┐
        ▼             ▼             ▼
  ┌───────────┐ ┌───────────┐ ┌───────────┐
  │ Follower 1│ │ Follower 2│ │ Follower 3│  ◀── Reads spread across these
  └───────────┘ └───────────┘ └───────────┘
```

> **Replication lag is the concrete cost.** Followers usually replicate *asynchronously* for performance, meaning a follower can be milliseconds (or, under load, much longer) behind the leader — a user who just wrote data and immediately reads it back from a follower might not see their own write yet ("read-your-own-writes" consistency, Part V.1). The fix is either routing a user's own immediate post-write reads to the leader specifically, or accepting the staleness window as a documented trade-off for the use case.

**Multi-leader (multi-master) replication** — more than one node accepts writes, replicating to each other — higher write availability (a region can keep accepting writes even if another region is unreachable), at the cost of needing to resolve write conflicts (two leaders accepting a conflicting write to the same record) — genuinely harder, and worth it mainly for multi-region systems where write latency to a single global leader would be unacceptable.

### Sharding (partitioning)

When even a well-replicated database's *write* capacity (which replication doesn't help — every leader-follower write still funnels through one leader) or total data size exceeds a single machine, **sharding** splits the data itself across multiple independent database servers, each holding a subset of the data.

**Horizontal sharding (the common meaning of "sharding")** — split rows across shards, typically by a **shard key** (e.g., `user_id`):

```
Shard 1: users with id 1–1,000,000
Shard 2: users with id 1,000,001–2,000,000
Shard 3: users with id 2,000,001–3,000,000
```

**Consistent hashing** — the standard technique for deciding which shard a given key belongs to, specifically chosen because it minimizes reshuffling when a shard is added or removed:

```
Naive approach: shard = hash(key) % number_of_shards
  Problem: adding ONE new shard changes `number_of_shards`, which changes
  the result of the modulo for almost EVERY key — nearly all data needs
  to move, all at once.

Consistent hashing: both shards and keys are placed on a conceptual
  ring (via hashing); a key belongs to the next shard clockwise from it.
  Adding a new shard only remaps the keys between it and the previous
  shard on the ring — a small, bounded fraction of keys move, not nearly all of them.
```

```
        Shard A (hash: 10)
              ●
         ╱         ╲
   key "x" (7)   Shard C (hash: 200)
    lands here ──▶ ●
         ╲         ╱
              ●
        Shard B (hash: 100)

Adding Shard D between A and C only remaps keys that fall in that arc —
everything between B and A, and between C and A, is untouched.
```

> **What sharding costs — and it's substantial.** Cross-shard queries (find all orders across every user — now scattered across every shard) and cross-shard transactions (the exact problem the Microservices guide's Saga pattern, Part III, was built to solve, but now *within* what used to be one database) become genuinely hard. Choose the shard key carefully — a poorly chosen key (e.g., sharding by signup date when most active users all signed up in the same recent window) creates **hot shards** that receive disproportionate load while others sit nearly idle, defeating the entire point of spreading load.

### CAP theorem and PACELC

**CAP theorem**: in the presence of a **network Partition** (P — some nodes can't talk to others, which *will* eventually happen in any distributed system), you must choose between **Consistency** (every read gets the most recent write, or an error) and **Availability** (every request gets a response, even if it might not be the latest data).

```
Network partition occurs — Node A and Node B can't communicate.
A client writes to Node A. A client then reads from Node B.

CP choice: Node B refuses to answer (or returns an error) until the
           partition heals, rather than risk returning stale data.
AP choice: Node B answers anyway, with whatever data it currently has —
           possibly stale, but the system stays responsive.
```

> **The theorem only applies *during* a partition — this is the most common misunderstanding.** When the network is healthy (the common case), a well-designed system can be both consistent and available simultaneously; CAP only forces a choice *during* the (hopefully rare, hopefully brief) window when a partition is actually happening. This is exactly what **PACELC** extends the theorem to address: **P**artition → choose **A**vailability or **C**onsistency (as above), **E**lse (no partition) → choose **L**atency or **C**onsistency — even with a healthy network, a system that wants strong consistency across replicas must wait for replication to confirm (added latency), while one that tolerates eventual consistency can respond immediately from any replica.

> **Why this matters when picking a database.** "CP" systems (traditional relational databases in a single-leader configuration, ZooKeeper, most configurations of MongoDB) prioritize correctness over uptime during a partition — right for financial data, inventory counts, anything where serving stale/wrong data is worse than an error. "AP" systems (Cassandra, DynamoDB by default) prioritize staying responsive — right for data where staleness is tolerable (a "like" count, a activity feed) but an outage is not.

### ACID vs. BASE

- **ACID** (Atomicity, Consistency, Isolation, Durability) — the traditional relational-database transaction guarantee: a transaction either fully happens or fully doesn't (atomicity), the database moves from one valid state to another (consistency), concurrent transactions don't interfere with each other (isolation), and a committed transaction survives a crash (durability). Strong guarantees, at the cost of coordination overhead that limits horizontal scale.
- **BASE** (Basically Available, Soft state, Eventual consistency) — the model many distributed/NoSQL systems embrace instead: the system stays available even under partial failure, state may not be immediately consistent everywhere, but will converge (become consistent) given enough time without new writes.

> **The trade-off in one sentence.** ACID gives you strong guarantees but doesn't scale horizontally without real difficulty (distributed transactions across shards are hard — see the Sharding section above); BASE scales horizontally naturally by relaxing the consistency guarantee. Neither is universally "better" — a payments ledger needs ACID; a social media "like" counter is a textbook BASE use case, since a like count being off by one for a few seconds has zero real consequence.

---

## Part V — Distributed Systems Concepts

### Consistency models

Beyond the binary "strong vs. eventual" framing, several specific, named consistency guarantees show up repeatedly in real system design:

| Model                          | Guarantee                                                                                                                                                                                                                           |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Strong consistency**   | Every read sees the most recent write, everywhere, immediately                                                                                                                                                                      |
| **Eventual consistency** | Given no new writes, all replicas*will* converge to the same value — eventually, with no bound on how long                                                                                                                       |
| **Read-your-own-writes** | A user always sees their*own* writes immediately, even if other users might see stale data briefly (a common, pragmatic middle ground — e.g., route a user's post-write reads to the leader specifically, as noted in Part IV.3) |
| **Causal consistency**   | If write A happened-before write B (B saw A's result before being made), every replica shows A before B — but unrelated, concurrent writes can be seen in different orders on different replicas                                   |

> **Why the middle-ground models exist.** Pure strong consistency and pure eventual consistency are both extremes; most real products actually want something like "read-your-own-writes" — a user who just posted a comment should see it immediately, but it's usually fine if a *different* user sees that comment 200ms later than another user does. Naming the specific guarantee your system actually needs (rather than defaulting to "strong consistency" out of caution, which costs real latency and availability) is a genuine design skill.

### Consensus: Paxos and Raft, at the level you need

Distributed systems frequently need multiple nodes to agree on a single value or ordering of events despite nodes failing or messages being delayed/lost — this is the **consensus problem**, and Paxos and Raft are the two dominant algorithms solving it (Raft was explicitly designed to be more understandable than Paxos, and has become the more common choice in new systems as a result — etcd, Consul, and most modern distributed databases use Raft or a close variant).

**The problem, stripped to its essence:** a cluster of nodes needs to agree on "what is the current leader" (or "what is the next entry in a replicated log") even when some nodes might be down or slow, and even when messages can be delayed — and the algorithm must guarantee that once a majority of nodes agree on a value, that agreement can never be silently overturned by a minority.

**Raft's core mechanism, at the level worth knowing for system design (not implementing it yourself):**

```
1. Nodes elect a LEADER via majority vote (a node that doesn't hear from
   a leader within a timeout starts an election).
2. All writes go through the leader, which replicates them to followers
   as log entries.
3. An entry is considered COMMITTED only once a MAJORITY of nodes have
   it in their log — this majority requirement is what survives a
   minority of nodes failing or being partitioned away.
4. If the leader dies, a new election happens among the remaining nodes.
```

> **Why this matters for system design without needing to implement it.** You will very rarely implement Paxos/Raft yourself — you'll use a system built on one (ZooKeeper, etcd, Consul for coordination/config; most modern distributed SQL databases like CockroachDB or Spanner for data replication). What matters is recognizing *when a design needs consensus at all*: anytime multiple nodes must agree on a single "source of truth" value — leader election, distributed configuration, a strongly consistent replicated log — that's a sign to reach for a proven consensus-based system rather than inventing an ad-hoc coordination mechanism, which is a well-documented source of subtle distributed-systems bugs.

### Distributed locking

Sometimes multiple processes/services need mutual exclusion — only one of them should perform some action at a time (only one instance should run a scheduled job, only one request should process a given payment). A distributed lock provides this across separate machines, typically built on top of Redis (`SET key value NX EX <ttl>` — set only if not already set, with an expiry) or a consensus system like ZooKeeper/etcd.

```js
// A minimal Redis-based distributed lock
const acquired = await redis.set(`lock:job:daily-report`, workerId, 'NX', 'EX', 30);
if (!acquired) return; // another worker already holds the lock — skip

try {
  await runDailyReport();
} finally {
  await redis.eval(releaseScript, 1, `lock:job:daily-report`, workerId); // release, only if we still own it
}
```

> **Why the TTL and the "only release if we still own it" check both matter.** Without a TTL, a worker that crashes while holding the lock leaves it locked forever, deadlocking every future run. Without the ownership check on release (a Lua script comparing the stored value to the worker's own ID before deleting), a worker whose lock already expired and was re-acquired by a *different* worker could accidentally release the new worker's lock, letting a third worker in concurrently — a subtle, classic distributed-locking bug.

### Message queues, recap

Async messaging (Kafka, RabbitMQ, SQS) — covered in full in the Microservices guide's Part II — is also a core system design tool outside of a microservices context specifically: it **decouples producers from consumers in time**, letting a system absorb a traffic spike (messages queue up rather than overwhelming a downstream service) and letting one part of the system fail temporarily without blocking the part producing the work.

> **The one system-design-specific framing worth adding here.** A queue is a **buffer that trades latency for durability and load-smoothing** — a request that goes through a queue is not processed synchronously/immediately, but it's far less likely to be lost or to overwhelm a downstream service during a spike. Anytime a design needs to handle bursty traffic against a downstream system with a roughly fixed processing capacity (image processing, sending emails, generating reports), a queue in between is usually the right answer.

---

## Part VI — Reliability & Performance Patterns

### Rate limiting algorithms

Beyond the concept (introduced in the Microservices guide's Part IV), the specific algorithm matters for correctness at the edges:

```
Token Bucket: a bucket holds up to N tokens, refilling at a fixed rate;
  each request consumes one token; empty bucket = reject. Allows
  short BURSTS up to the bucket's capacity, which is often desirable
  (a user's occasional quick flurry of clicks shouldn't be punished
  the same as sustained abuse).

Leaky Bucket: requests enter a queue and are processed at a FIXED,
  constant rate regardless of how bursty the input is — smooths
  traffic completely, but a burst just waits longer instead of
  being processed faster.

Fixed Window: count requests in fixed time windows (e.g., per-minute);
  simple, but allows up to 2x the intended rate right at a window
  boundary (a burst at 11:59:59 and another at 12:00:01 both fit
  within their own window's limit, doubling the effective rate
  for that 2-second span).

Sliding Window Log: track the exact timestamp of every request in
  the recent window; accurate, but memory cost grows with request
  volume — the log itself can become the bottleneck at high scale.

Sliding Window Counter: approximate the sliding window using a
  weighted average of the current and previous fixed windows —
  the practical compromise most production rate limiters actually use.
```

> **Why the fixed-window boundary bug matters concretely.** A rate limit of "100 requests/minute" implemented naively with fixed windows lets a client send 100 requests at 11:59:59 and another 100 at 12:00:00 — 200 requests in a two-second span, double the intended limit. This is a genuinely common, real bug — the sliding window counter approach exists specifically to close this gap without paying the sliding window log's full memory cost.

### Health checks and failover

Covered in depth for the microservices context in the Microservices guide's Part V (liveness vs. readiness) — the system-design-level addition is **failover**: what happens at the infrastructure level once a health check fails.

```
Load Balancer polls each backend's /health endpoint every N seconds
         │
         ▼
   Backend fails 3 consecutive checks → marked UNHEALTHY
         │
         ▼
   Load Balancer stops routing traffic to it (but keeps polling)
         │
         ▼
   Backend passes health checks again → automatically re-added to rotation
```

For a database's leader specifically, failover is more involved: a **failover process** must detect the leader is actually down (not just slow — a false positive here can cause a dangerous "split brain" where two nodes both believe they're the leader), promote a follower to the new leader, and redirect write traffic — this is precisely the kind of coordination problem consensus algorithms (Part V.2) are built to handle correctly.

### Idempotency

An operation is **idempotent** if performing it multiple times has the same effect as performing it once — critical in any distributed system where retries (Part IV of the Microservices guide) are a normal, expected occurrence, because a retry might be re-sending a request that actually *did* succeed the first time, but whose success response was lost in transit.

```js
// NOT idempotent — retrying this after a lost response DOUBLE-CHARGES the customer
app.post('/charge', async (req, res) => {
  await chargeCard(req.body.amount, req.body.cardId);
  res.send({ status: 'charged' });
});

// Idempotent — a client-generated idempotency key lets the server recognize
// and safely ignore a retry of a request it already successfully processed
app.post('/charge', async (req, res) => {
  const existing = await db.query(
    'SELECT * FROM charges WHERE idempotency_key = ?', [req.headers['idempotency-key']]
  );
  if (existing) return res.send(existing); // already processed — return the SAME result, don't charge again

  const charge = await chargeCard(req.body.amount, req.body.cardId);
  await db.query(
    'INSERT INTO charges (idempotency_key, result) VALUES (?, ?)',
    [req.headers['idempotency-key'], charge]
  );
  res.send(charge);
});
```

> **Why this matters everywhere retries happen.** Every retry pattern in this guide (network retries, message queue redelivery, a client resubmitting a form after a timeout with no visible confirmation) is only safe to actually retry if the underlying operation is idempotent — designing for idempotency from the start (client-generated idempotency keys are the standard technique for genuinely non-idempotent operations like payments) is what makes "just retry on failure" a safe default instead of a data-corruption risk.

---

## Part VII — Storage & Search

### Blob storage

Large, unstructured binary data (images, videos, backups, log archives) doesn't belong in a relational database's row storage — **object/blob storage** (Amazon S3, Google Cloud Storage — covered as a service in the DevOps guide's Part V) is purpose-built for this: durable, cheap at scale, accessed by key rather than queried relationally, and almost always paired with a CDN (Part II.3) in front of it for actual serving to end users.

> **Why not just store files in the database.** Relational databases are optimized for structured, relatively small rows with fast transactional access — storing multi-megabyte blobs directly in database rows bloats the database's own storage and backup size, slows down unrelated queries competing for the same disk I/O, and gains nothing over a purpose-built object store, which is both cheaper and better suited to the access pattern (fetch-by-key, rarely queried by content). The standard pattern is: store the *file* in blob storage, store only its *URL/key* as a column in the relational database.

### Search: inverted indexes

A B-tree index (Part IV.2) answers "find rows where `column = value`" efficiently — it does not efficiently answer "find documents containing the word 'scalable' anywhere in a large text field." Full-text search needs a different structure: an **inverted index**, which maps each word to the list of documents containing it (the inverse of a normal document → words mapping):

```
Forward index (normal):        Inverted index (search-optimized):
doc1: "the system is scalable"  "system":    [doc1, doc3]
doc2: "a fast database engine"  "scalable":  [doc1]
doc3: "a scalable system design" "fast":      [doc2]
                                 "database":  [doc2]
                                 "design":    [doc3]

Query "scalable system" → intersect the postings lists for "scalable"
and "system" → [doc1] ∩ [doc1, doc3] = [doc1] — found instantly,
without scanning every document's text.
```

Elasticsearch (built on Apache Lucene) is the standard tool implementing this at scale, typically run as a separate search-specific store fed by events from the primary database (another real-world instance of the CQRS pattern from the Microservices guide's Part III) rather than trying to make the primary relational database itself do full-text search well.

---

## Part VIII — Designing Real Systems

Each of the following applies Parts I–VII to a specific, commonly-asked design — the point is the reasoning chain (requirements → estimation → component choice → trade-off), not memorizing the final diagram.

### Design a URL shortener

**Requirements**: shorten a long URL to a short code, redirect the short code back to the original, handle very high read (redirect) volume relative to writes.

**Estimation**: 100M new URLs/month ≈ 40 writes/sec average; redirects typically outnumber creates by 100:1 → ≈ 4,000 reads/sec.

**Design**:

```
POST /shorten { longUrl } → generate a short code → store {code: longUrl} → return short URL
GET /{code} → look up longUrl by code → HTTP 302 redirect
```

- **Code generation**: base62-encode an auto-incrementing ID (simple, guaranteed unique, no collision-checking needed) rather than hashing the URL (hash collisions require detection/retry logic for little benefit here).
- **Storage**: a simple key-value store (Part IV.1's NoSQL case) — the access pattern is purely by-key lookup, no relational structure needed. Given the 100:1 read:write ratio, aggressively cache the code→URL mapping (Part III.2) — this single cache is likely the highest-leverage component in the whole design.
- **Reads at this volume**: comfortably served from a cache-aside layer in front of the key-value store; a CDN doesn't directly help here since the redirect target is dynamic per-code, but the *lookup* being cache-backed achieves the same latency goal.

### Design a rate limiter

**Requirements**: limit each user/API-key to N requests per time window, work correctly across multiple application server instances (not just limit each instance independently).

**Design**: the algorithm choice is Part VI.1; the system-design-specific decision is **where the counter state lives**. Each application instance can't keep its own independent count (a user could get N× the limit by hitting N different instances) — the counter must be centralized, typically in Redis, since it needs to be fast (checked on every request) and shared:

```js
const key = `ratelimit:${userId}:${currentWindow()}`;
const count = await redis.incr(key);
if (count === 1) await redis.expire(key, windowSizeSeconds);
if (count > limit) return res.status(429).send();
```

> This is the fixed-window algorithm from Part VI.1 — worth explicitly naming its known boundary weakness in an interview setting and proposing the sliding-window-counter upgrade if precision matters more than simplicity here.

### Design a news feed (Twitter/Instagram)

**Requirements**: users follow other users; a user's feed shows recent posts from everyone they follow, roughly reverse-chronological (or ranked).

**The central trade-off — fan-out on write vs. fan-out on read:**

```
Fan-out on WRITE (push): when a user posts, immediately push that post
  into the PRE-COMPUTED feed of every one of their followers.
  → Reading a feed is instant (just read the pre-computed list).
  → Writing is expensive for users with millions of followers
    (one post = millions of feed insertions).

Fan-out on READ (pull): a user's feed is computed ON DEMAND at read
  time, by fetching recent posts from everyone they follow and merging.
  → Writing is cheap (one post = one insert, nothing else).
  → Reading is expensive — must fetch and merge from every followed
    user, every single time the feed is viewed.
```

> **The real answer is both, split by follower count** — this exact hybrid is documented in Part IX.1 (Twitter's real architecture): fan-out-on-write for the vast majority of users (cheap, since most users have a normal-sized follower count), with a fallback to fan-out-on-read specifically for celebrity accounts with millions of followers, where a write-time fan-out would be prohibitively expensive. This hybrid is the single most-cited real trade-off in feed system design interviews specifically because it demonstrates recognizing that a uniform solution is wrong when the actual data (follower count) has a genuinely skewed, long-tail distribution.

### Design a chat system (WhatsApp)

**Requirements**: real-time message delivery, delivery/read receipts, works when the recipient is offline.

**Design**:

- **Real-time delivery**: a persistent connection per online client — WebSockets (not repeated HTTP polling, which wastes resources checking "any new messages?" constantly) — with each server instance tracking which of *its* connected clients are online, and a message broker (Part V.4) routing a message to whichever specific server instance holds the recipient's live connection.
- **Offline delivery**: messages for an offline recipient are persisted to a database queue and delivered (pushed over their WebSocket) the moment they reconnect — this durability requirement is exactly why chat systems can't rely on the WebSocket connection alone as the source of truth; the database write must be considered the actual "message sent" moment, with WebSocket delivery as a secondary, best-effort fast path.
- **Delivery/read receipts**: modeled as their own small state machine per message (`sent` → `delivered` → `read`), each transition itself a small, idempotent (Part VI.3) event.

> **Why WebSocket routing is the genuinely hard part.** Unlike a stateless HTTP API (Part III.1), a WebSocket connection is inherently stateful — a specific server instance holds a specific open connection to a specific user. Routing an incoming message to "wherever that recipient's connection currently is" requires either a shared registry (which instance holds which user's connection, similar in spirit to Part II.4's service discovery) or a broker fan-out where every instance subscribes and simply drops messages meant for users it doesn't currently hold — a direct, practical instance of the stateful-service problem from Part III.1.

### Design a distributed cache

**Requirements**: cache large amounts of data across many machines (more than fits on one), with fast lookups and graceful handling of individual cache node failures.

**Design**: this is Part IV.4's sharding applied specifically to a cache — a key is mapped to one of many cache nodes via **consistent hashing** (so adding/removing a cache node only reshuffles a small fraction of keys, not the whole cache), with each node holding an independent in-memory hash map for its assigned key range.

```
get(key) → hash(key) → determine which cache node owns this key (via consistent hashing ring)
         → forward request directly to that node → return value or MISS
```

> **What happens when a cache node dies.** With consistent hashing, only the keys that node owned are lost (they simply become cache misses, falling back to the underlying database per Part III.2's cache-aside pattern) — the rest of the cache is entirely unaffected, which is the direct payoff of choosing consistent hashing over a naive `hash % N` scheme (Part IV.4) for exactly this failure scenario.

---

## Part IX — Real Architectures

### Twitter: fan-out and the timeline problem

**The problem, concretely.** Twitter's own engineering writing has documented that a small fraction of accounts (celebrities, large media accounts) have follower counts in the tens of millions, while the overwhelming majority of accounts have a few hundred followers or fewer — a genuinely extreme, long-tailed distribution. A single design choice (Part VIII.3's fan-out-on-write vs. fan-out-on-read) applied uniformly breaks badly at one end of that distribution or the other.

**The documented solution**: Twitter uses fan-out-on-write for the vast majority of accounts — a tweet is immediately pushed into the pre-computed timeline of every follower, making timeline reads for ordinary users fast and cheap. For accounts above a follower-count threshold, the system instead falls back to computing that account's contribution to a follower's timeline at read time, merging it in on demand, specifically to avoid a single celebrity tweet triggering tens of millions of synchronous timeline-insertion writes.

> **The lesson.** This is the clearest real-world validation of Part VIII.3's hybrid answer — and more generally, of a pattern worth generalizing beyond feeds: **when a system's real-world input distribution is genuinely skewed (a small number of outliers behave completely differently from the median case), the right design frequently isn't one algorithm — it's a threshold-based hybrid of two**, each suited to one end of the distribution.

### Dropbox: how file sync actually works

**The problem.** Syncing files across devices needs to (a) detect that a file changed, (b) transfer only what's necessary (not the whole file every time a byte changes), and (c) resolve conflicts when the same file is edited on two devices while offline.

**The documented approach (from Dropbox's own engineering writing)**: files are broken into fixed-size **blocks**; when a file changes, Dropbox's client computes which blocks actually differ from the last synced version and uploads **only those changed blocks**, not the entire file — directly analogous to `rsync`'s delta-transfer approach (covered in the Linux guide's Part IV). Metadata about which blocks compose which file version is tracked centrally, allowing the same unchanged block to be referenced by multiple file versions (or even multiple users' identical files) without re-storing duplicate data.

> **The lesson.** This is a real-world, large-scale application of a simple idea also seen in Part IV's data layer: **don't move/store more than what actually changed**. The block-level diffing is conceptually the same insight as a database write-ahead log only persisting changes, or `rsync` only transferring changed bytes — recognizing "we're about to move/store the whole thing when only a fraction of it actually changed" is a recurring, generalizable optimization across very different systems.

### Netflix: CDN + microservices at global scale

**The architecture, at a high level (documented extensively in Netflix's own engineering blog, and touched on for its resilience-tooling angle in both the DevOps guide's chaos engineering section and the Microservices guide's Part VIII).** Netflix operates its own purpose-built CDN (**Open Connect**) with servers placed directly inside ISP networks around the world specifically to serve video content (the overwhelming majority of their total bandwidth) from as close to the end user as possible — the same CDN principle from Part II.3, taken to the extreme of Netflix operating the edge infrastructure itself rather than using a third-party CDN, because video streaming's bandwidth and latency requirements at their scale justified the investment.

Everything *other* than the actual video bytes — the API layer, recommendations, user profiles, billing — runs as the microservices architecture covered in the companion Microservices guide, backed by the resilience patterns (Part VI here, and the Microservices guide's Part IV) that let a single backend service's failure degrade gracefully (e.g., falling back to a generic, non-personalized recommendation list if the personalization service is unavailable) rather than taking down video playback entirely.

> **The lesson.** Netflix's architecture is a real-world illustration of a principle worth stating explicitly: **different parts of the same system can and should make different trade-offs**. Video delivery is CDN-and-bandwidth-optimized because that's where the actual bottleneck and cost live; the API/business-logic layer is optimized for independent team deployability via microservices, because that's where *that* layer's actual bottleneck (organizational coordination, per the Microservices guide's Part I) lives. There is no single "Netflix architecture" — there's a set of locally-appropriate decisions for each subsystem's actual constraint.

---

## Part X — Roadmap & Practice

### A learning roadmap

**Weeks 1–2 — foundations and estimation.** Drill back-of-the-envelope estimation (Part I.4) until it's fast and automatic; practice the vertical/horizontal scaling trade-off table until you can argue both sides for a given scenario.

**Weeks 3–4 — networking and caching.** Build a mental model solid enough to draw, from memory, the full path of a request from DNS through a CDN, load balancer, and application-level cache; implement cache-aside and write-through against a real Redis instance and observe the latency difference yourself.

**Weeks 5–7 — the data layer.** Set up real replication (leader-follower) between two Postgres instances and deliberately observe replication lag under load; implement a toy consistent-hashing ring in code (this is the single best way to actually understand why it beats naive modulo sharding).

**Weeks 8–9 — distributed systems concepts.** Read a Raft visualization/simulation (several interactive ones exist online) until leader election and log replication feel concrete, not abstract; implement the distributed-locking pattern from Part V.3 against real Redis and deliberately trigger the "lock expired and was stolen" bug to see why the ownership check matters.

**Weeks 10–11 — apply it.** Work through all five Part VIII designs from scratch, on a whiteboard, timed to 30–45 minutes each, out loud — the "explain your reasoning as you go" habit matters as much as the final diagram.

**Week 12 — real architectures & mocks.** Read Part IX's three cases in full elsewhere (Twitter's, Dropbox's, and Netflix's own engineering blogs go far deeper than this guide's summaries) and run 2–3 timed mock system design interviews with a peer.

### Question bank

A working list of commonly-asked system design problems, roughly ordered by typical difficulty — treat each as a fresh application of Parts I–VII, not a memorization target:

**Foundational**

- [ ] Design a URL shortener (Part VIII.1)
- [ ] Design a rate limiter (Part VIII.2)
- [ ] Design a key-value store
- [ ] Design a parking garage / ticketing system (a good pure object-modeling warm-up, less about distributed systems)

**Intermediate**

- [ ] Design a news feed (Part VIII.3)
- [ ] Design a distributed cache (Part VIII.5)
- [ ] Design a web crawler
- [ ] Design an autocomplete/typeahead system
- [ ] Design a notification system (push, email, SMS fan-out)
- [ ] Design a ride-sharing dispatch system (Uber/Lyft matching)

**Advanced**

- [ ] Design a chat system (Part VIII.4)
- [ ] Design a distributed file storage system (Dropbox/Google Drive — Part IX.2)
- [ ] Design a video streaming platform (Netflix/YouTube — Part IX.3)
- [ ] Design a distributed job scheduler
- [ ] Design a payment processing system (idempotency, Part VI.3, is central here)
- [ ] Design a search engine's ranking/indexing pipeline (Part VII.2)

### Projects to build, in order

**01. A load-balanced, stateless API**
Deploy the same simple API to 3 instances behind a load balancer (Nginx or a cloud load balancer); externalize any session state to Redis; kill one instance under load and confirm the others absorb it seamlessly.
*Skills: Part II.2, Part III.1.*

**02. A cache-aside layer with measured impact**
Add Redis cache-aside in front of a real database query; benchmark request latency with and without the cache under repeated load to see the actual numbers, not just the theory.
*Skills: Part III.2.*

**03. Replicated Postgres with observed lag**
Set up one leader and two followers; write continuously to the leader while reading from a follower, and log the actual replication lag under increasing write load.
*Skills: Part IV.3.*

**04. A toy consistent-hashing ring**
Implement consistent hashing from scratch (no library) for a simulated set of cache nodes; add and remove a node and measure what fraction of keys actually moved, compared to naive `hash % N`.
*Skills: Part IV.4.*

**05. A correctly-idempotent payment endpoint**
Build an endpoint that accepts a client-generated idempotency key, and write a test that fires the identical request 5 times concurrently, asserting only one actual charge occurs.
*Skills: Part VI.3.*

**06. A rate limiter with the boundary bug demonstrated**
Implement fixed-window rate limiting, then write a test that deliberately straddles a window boundary and shows the 2x burst bug from Part VI.1; fix it with a sliding window counter and re-run the same test.
*Skills: Part VI.1.*

**07. Capstone: a hybrid-fanout mini feed**
Build a small social feed where most users get fan-out-on-write, but accounts above a configurable follower threshold fall back to fan-out-on-read; load-test both paths and confirm the hybrid actually avoids the write-amplification problem a pure fan-out-on-write design would hit.
*Skills: Part VIII.3, Part IX.1, and effectively every earlier part combined.*

---

*This guide pairs directly with the companion Microservices, DevOps, and Linux tutorials — service boundaries, message queues, and resilience patterns are covered in full depth in the Microservices guide; Kubernetes, observability, and infrastructure concerns in the DevOps guide; this guide focuses specifically on the scaling, data, and distributed-systems reasoning that sits underneath both.*
