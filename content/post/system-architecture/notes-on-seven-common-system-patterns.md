---
title: "Notes on 7 common System Patterns"
date: 2026-02-01
featured: false
draft: true
toc: true
tags:
  - SysAdmin
  - system-architecture

---

## Ambassador

Ambassador = a local proxy that “represents” your app when it talks to the outside world.

The application only ever talks to `localhost` (or a local Unix socket).

The ambassador handles the complexities, such as:

* mTLS / cert rotation
* retries / timeouts
* circuit breaking / outlier detection
* auth headers / token injection
* rate limiting
* observability (access logs, traces)

protocol translation (e.g., HTTP/1.1 ↔ HTTP/2, gRPC)

Ambassador pattern in Kubernetes, chapter 17 (Bilgin Ibryam and Huß,
2019)[\[1\]](/post/system-architecture/notes-on-seven-common-system-patterns/#references)

## Circuit Breaker

Circuit Breaker is the “stop calling the thing that’s currently failing” pattern.

It sits between your service and a dependency (HTTP API, DB proxy, gRPC upstream, etc.) and prevents cascading failure
by failing fast when the downstream is unhealthy, then probing for recovery.

States (mental model)

Closed: calls flow normally. You’re watching error/latency rates.

Open: you block calls immediately (often return a cached value or a controlled error).

Half-open: you allow a small number of “test” calls; if they succeed, close again; if they fail, open again.


## CQRS (Command Query Responsibility Segregation)


## Event sourcing

ex: git Version Control


## Leader Election

ZK
## Pub/Sub

NewsPaper delivery 
Async message delivery

## Sharding

MongoDB, Cassandra

## Strangler Fake 

Strangler Pattern is useful to replacing legacy services gradyually

## References

1. Bilgin Ibryam and Huß, R. (2019). Kubernetes Patterns. [online] ‘O’Reilly Media, Inc.’ Available
   at: https://www.oreilly.com/library/view/kubernetes-patterns/9781492050278/ch17.html [Accessed 2 Feb. 2026].
2. [Top 7 Most used Distributed System Patterns](https://youtu.be/nH4qjmP2KEE?si=jIePLXDGfc3pVYSe)