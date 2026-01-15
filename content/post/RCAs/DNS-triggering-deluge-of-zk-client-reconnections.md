---
title: "Low TTLs caused ZK client reconnect storm"
date: 2026-01-14T10:00:10Z
draft: true
featured: false
toc: false

categories:
  - Technology
  - Opinions
  - Debug
  
tags:
  - ZooKeeper
  - DNS
  - Troubleshooting
series:
  - rcas
---

## Abstract

ZooKeeper service relies on long lived client connections by design. 


Chapter 3 Creating a Session. A Session that is established with one Server will move to another if its connection is broken.

Important to define a session. TODO

As Long as a session is alive the handle will remain valid, and the Client will continually tra to keep an active connection. 

If the Client handle is closed, it "tells" the server ti kill the session. 

If ZooKeeper (Server) "decides" as client has died, it will invalidate the session. If the client later retries a connection with a handle corresponding to a invalidated session. 
The Server infromer the client the session is no longer valid. The handle wil fail all operations.



### Important Definition

Session

watcher

Leader tracks session in Quorum mode 

Keeping a session alive
a learner (client or non-leader node) responds to a heartbeat.

A heart beat is either a reply to a PING message or a new request

A ping is sent every half a tick (unit of time in milliseconds used by ZooKeeper)

Failing the above a session is sent to the expiry queue, which the leader will devide in buckets according to the expiration interval. I won't go further as it's not relevant for this troubleshoot scenario description


## References

* [ZK Sessions](https://zookeeper.apache.org/doc/current/zookeeperProgrammers.html#ch_zkSessions)
* [ZNodes](https://zookeeper.apache.org/doc/current/zookeeperProgrammers.html#sc_zkDataModel_znodes)
* Junqueira, F. and Reed, B., 2013. ZooKeeper: distributed process coordination. " O'Reilly Media, Inc.".