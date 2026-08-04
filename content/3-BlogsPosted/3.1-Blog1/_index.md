---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# [Technical Corner] Building Multi-Region REST APIs with Aurora DSQL: Handling Data Conflict and Credential Management

Recently, while working on system architectures, I read an insightful article on the **AWS Database Blog** about combining **Spring Boot** and **Amazon Aurora DSQL**. The problem statement is: *How to run REST APIs across multiple regions (Multi-Region Active-Active) without database synchronization issues?*

Backend developers know the classic challenge when two requests from different regions update the same data row at the same time (e.g., deducting product stock). Traditional locking mechanisms easily cause connection bottlenecks or even Deadlocks, not to mention the complexity of syncing database passwords between nodes.

Combining **Aurora DSQL** with **Spring Boot** resolves this problem through three key mechanisms:

1. **No Static Passwords**: The architecture uses `DsqlConnector` which automatically authenticates using IAM Roles, handles token refreshes, and enforces TLS encryption. Developers no longer need to hardcode passwords or manage complex secrets.
2. **Conflict Resolution using Optimistic Concurrency Control (OCC)**: DSQL does not use locks. When a conflict occurs during commit, one request succeeds while the other fails with a `40001` SQL state error.
3. **Spring Boot and HikariCP Integration**: Instead of throwing a `500` error to users, we can handle it gracefully in the background:
   * Configure `DsqlExceptionOverride` to tell **HikariCP** to keep the connection open when encountering error `40001` since the connection itself is still healthy.
   * Use Spring's `@Retryable` annotation to catch the error and automatically retry the transaction using **Exponential Backoff**. As a result, the client still receives a clean `HTTP 200 OK` response.

---

### Multi-Region Architecture Diagram

![Aurora DSQL Multi-Region Architecture](/images/3-BlogsPosted/3.1-Blog1/aurora-dsql-architecture.png)
*Figure 1: Spring Boot active-active multi-region deployment with Aurora DSQL.*

---

### Summary

This architecture allows you to focus on business logic rather than database infrastructure. If a region goes down, Route 53 routes traffic to the other region, and the application continues to run normally without any code changes.

For high-traffic systems, have you applied this OCC + Spring Retry mechanism in production? How does the connection pool perform in practice? Share your thoughts below! 👇

---

* **Original Reference Link**: [AWS Database Blog](https://aws.amazon.com/blogs/database/build-a-spring-boot-rest-api-with-amazon-aurora-dsql/)
* **Facebook Post Link**: [AWS Study Group Facebook Post](https://www.facebook.com/photo?fbid=2093845508234493&set=gm.2199940367437590&idorvanity=660548818043427)