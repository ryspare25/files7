# Redis Explained: A Deep Dive

## What is Redis?

Redis (REmote DIctionary Server) is an in-memory data structure store used as:

- Cache
- Database
- Message broker
- Queue
- Pub/Sub system
- Distributed lock manager
- Real-time analytics engine

Unlike traditional databases that primarily store data on disk, Redis keeps most data in RAM, making it extremely fast.

Typical latency:
- Redis: microseconds
- Traditional databases: milliseconds

That difference sounds small, but at scale it is enormous.

---

# The Core Idea

Imagine a giant hash table living in memory.

Instead of:

```text
Application -> Disk Database -> Disk Read
```

Redis does:

```text
Application -> RAM
```

RAM is thousands of times faster than disk.

---

# How Redis Stores Data

Redis stores:

```text
Key -> Value
```

Example:

```redis
SET user:1001 "Jade"
```

Internally:

```text
"user:1001" -> "Jade"
```

Retrieve:

```redis
GET user:1001
```

Result:

```text
Jade
```

---

# Why Redis is Fast

## 1. In-Memory Storage

Everything lives in RAM.

No spinning disks.
Minimal SSD access.

## 2. Single-Threaded Command Execution

Redis traditionally processes commands in a single execution path.

Benefits:

- No locking
- No race conditions inside command execution
- Minimal synchronization overhead

Example:

```text
Client A -> INCR counter
Client B -> INCR counter
```

Redis processes:

```text
INCR
INCR
```

One at a time.

Atomic by default.

## 3. Efficient Data Structures

Redis isn't just a key-value store.

It provides optimized structures.

---

# Redis Data Structures

## String

Most common.

```redis
SET name "Alice"
GET name
```

Use cases:

- Cache values
- Session tokens
- Configuration

---

## Lists

Ordered collection.

```redis
LPUSH tasks "task1"
LPUSH tasks "task2"
```

Result:

```text
tasks:
task2
task1
```

Use cases:

- Queues
- Job scheduling
- Activity feeds

---

## Sets

Unique items.

```redis
SADD online_users user1
SADD online_users user2
```

No duplicates.

Use cases:

- Tags
- Membership
- Permissions

---

## Sorted Sets

Each item has a score.

```redis
ZADD leaderboard 100 alice
ZADD leaderboard 250 bob
```

Result:

```text
bob 250
alice 100
```

Use cases:

- Rankings
- Leaderboards
- Recommendations

---

## Hashes

Object-like storage.

```redis
HSET user:1 name Alice age 28
```

Retrieve:

```redis
HGET user:1 name
```

Use cases:

- User profiles
- Product information

---

## Streams

Log-like structure.

```redis
XADD orders * id 1 amount 100
```

Use cases:

- Event sourcing
- Messaging
- Analytics pipelines

---

# Redis as a Cache

Most common use.

Without cache:

```text
User Request
    ↓
Application
    ↓
Database
```

With Redis:

```text
User Request
    ↓
Application
    ↓
Redis Cache
    ↓
Database (only if cache miss)
```

---

# Cache Hit vs Cache Miss

## Cache Hit

Data exists.

```text
Redis contains user:1001
```

Response immediately.

## Cache Miss

Data absent.

Application:

1. Reads database
2. Returns result
3. Stores in Redis

Next request becomes a hit.

---

# Expiration (TTL)

Redis can automatically delete keys.

```redis
SET session123 abc EX 3600
```

Meaning:

```text
Expire after 1 hour
```

Perfect for:

- Sessions
- Authentication tokens
- Temporary data

---

# Persistence

Redis lives in RAM.

But RAM disappears after power loss.

Redis provides persistence.

---

## RDB Snapshots

Periodically saves memory to disk.

Example:

```text
Every 5 minutes
```

Create:

```text
dump.rdb
```

Advantages:

- Compact
- Fast recovery

Disadvantages:

- Possible recent data loss

---

## AOF

Append Only File.

Every write:

```text
SET x 10
SET y 20
DEL z
```

Recorded sequentially.

Recovery:

```text
Replay commands
```

Advantages:

- Better durability

Disadvantages:

- Larger files

---

# Replication

One master.

Many replicas.

```text
Master
 /   \
R1   R2
```

Writes:

```text
Master
```

Reads:

```text
Master or Replica
```

Benefits:

- Scalability
- High availability

---

# Redis Sentinel

Sentinel monitors Redis instances.

Responsibilities:

- Health checks
- Failure detection
- Automatic failover

Example:

```text
Master dies
↓
Sentinel promotes replica
↓
System continues
```

---

# Redis Cluster

For huge datasets.

Data split across nodes.

```text
Node1
Node2
Node3
```

Redis uses:

```text
16384 hash slots
```

Keys distributed among slots.

Benefits:

- Horizontal scaling
- Massive throughput

---

# Pub/Sub

Publisher:

```redis
PUBLISH news "Hello"
```

Subscriber:

```redis
SUBSCRIBE news
```

All subscribers receive message.

Use cases:

- Chat systems
- Notifications
- Real-time updates

---

# Distributed Locks

Prevent multiple servers doing same work.

Acquire:

```redis
SET lock:job123 token NX EX 30
```

Meaning:

```text
Only create if absent
Expire after 30 seconds
```

Used for:

- Scheduled jobs
- Preventing duplicate execution

---

# Rate Limiting

Example:

```redis
INCR api:user1
EXPIRE api:user1 60
```

Track requests per minute.

Applications:

- API protection
- Abuse prevention

---

# Real-World Example

User logs in.

Application:

```text
1. Verify password
2. Generate session token
3. Store token in Redis
```

```redis
SET session:abc123 user1001 EX 3600
```

Future requests:

```text
Token -> Redis lookup
```

Extremely fast.

---

# Redis vs SQL Database

| Feature | Redis | SQL |
|----------|--------|------|
| Storage | RAM | Disk |
| Speed | Very Fast | Fast |
| Joins | No | Yes |
| Transactions | Limited | Strong |
| Complex Queries | Limited | Excellent |
| Primary Use | Cache | Source of Truth |

Common architecture:

```text
Application
     |
     +-- Redis
     |
     +-- PostgreSQL
```

Redis accelerates.

PostgreSQL persists.

---

# Redis Mental Model

Think of Redis as:

```text
A giant shared RAM computer
available to all servers
over the network.
```

Instead of each application maintaining its own memory:

```text
Server A memory
Server B memory
Server C memory
```

You get:

```text
Redis memory
```

Shared by everyone.

---

# Interview Questions

## Why is Redis fast?

- RAM
- Efficient structures
- Minimal overhead
- Single-threaded execution model

## When should Redis not be the primary database?

- Complex joins
- Heavy relational workloads
- Long-term durable storage requirements

## What is TTL?

Time To Live.
Automatic expiration.

## Difference between RDB and AOF?

RDB:
- Snapshot

AOF:
- Command log

## What is a cache stampede?

Many requests miss cache simultaneously and overload database.

Solutions:

- Mutex locking
- Cache warming
- Staggered expiration

---

# Final Summary

Redis is best understood as:

1. An ultra-fast in-memory data store.
2. A cache that protects databases.
3. A shared memory layer for distributed systems.
4. A toolbox of powerful data structures.
5. A foundational component in modern web-scale architectures.

Most large systems use Redis somewhere:

- Social media
- Banking
- Gaming
- E-commerce
- Streaming platforms
- Real-time analytics

If SQL databases are the long-term memory of a system, Redis is the short-term working memory that makes everything feel instant.
