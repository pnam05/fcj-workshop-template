---
title: "Blog 1"
date: 2026-06-20
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# CONNECTION EXHAUSTION WHEN COMBINING AWS LAMBDA WITH RDS — AND HOW AMAZON RDS PROXY SOLVES IT

### 1. Introduction

In Operating System and Database Management System courses, **Connection Pooling** is a classic technique implemented in source code to optimize resource reuse. However, when shifting this model to a **Serverless** architecture on cloud computing, traditional operational principles reveal critical architectural bottlenecks.

This article provides an in-depth analysis of **Connection Exhaustion** when integrating AWS Lambda with Relational Database Management Systems (RDBMS), and clarifies how **Amazon RDS Proxy** thoroughly solves this challenge.

---

### 2. Root Cause: Conflict Between Two Architectural Models

The **Serverless (AWS Lambda)** model and **Relational Databases (Amazon RDS** such as PostgreSQL or MySQL) are designed based on opposing operational philosophies:

- **AWS Lambda (Elastic Scaling & Stateless):** Capable of automatically scaling out from 0 to thousands of execution environments in milliseconds during sudden traffic surges. These instances are completely stateless and short-lived.

- **Amazon RDS (Resource-Intensive Initialization & Fixed):** Establishing each connection to RDS incurs significant resource overhead. For example, in PostgreSQL, each new connection requires the OS to allocate a separate process, consuming an average of **~10MB of RAM**. The maximum connection limit (`max_connections`) is strictly bounded from a few hundred to a thousand, directly depending on the server's RAM configuration.

**Incident Scenario:** When the system experiences a sudden spike in traffic, API Gateway triggers 2,000 Lambda functions concurrently. Each Lambda function automatically initializes a new connection to the database. If RDS only supports a maximum of 500 concurrent connections, the system will immediately throw `Too many connections` errors, rejecting queries and causing a **cascading failure** across the entire system.

---

### 3. Limitations of Traditional Connection Pooling

In traditional application models, engineers typically use libraries such as **pg-pool** (Node.js) or **HikariCP** (Java) to maintain a set of open connections. However, this solution **cannot work effectively in an AWS Lambda environment**.

Because each Lambda function runs in an isolated execution environment, they cannot share memory space with each other. When 2,000 Lambda instances are launched simultaneously, the system creates **2,000 independent Connection Pools**, each attempting to maintain its own set of connections. Rather than relieving database load, this **multiplies connection pressure exponentially**.

---

### 4. The Solution: Amazon RDS Proxy Intermediate Layer

To overcome this bottleneck, AWS provides **Amazon RDS Proxy** — a fully managed database proxy service that acts as a mediating layer between AWS Lambda and Amazon RDS.

The orchestration mechanism of RDS Proxy is based on three main technical pillars:

#### 4.1. Centralized Connection Pooling & Multiplexing

Instead of allowing Lambda functions to connect directly to the database, all traffic is routed through RDS Proxy. Here, Proxy maintains a **warm connection pool** to RDS. When a Lambda instance sends a query, Proxy temporarily assigns an idle connection from the pool, executes the SQL command, returns the result, and reclaims the connection back to the pool to serve subsequent requests.

This **multiplexing** technique allows **thousands of concurrent Lambda functions to efficiently share a small pool of actual DB connections**.

#### 4.2. Optimized Failover Process

In distributed systems, when the primary database server fails, AWS triggers a failover mechanism to the standby server. This process typically takes **30–60 seconds** and disrupts existing network connections.

When integrated with RDS Proxy, the service actively buffers queries from Lambda in a queue and automatically reroutes them to the new DB instance as soon as recovery completes. As a result, the application only experiences a slight increase in response latency rather than **connection errors**.

#### 4.3. Enhancing Security with IAM Authentication

Storing database credentials as plaintext in environment variables presents significant security risks. RDS Proxy allows Lambda functions to authenticate using AWS **IAM Roles**. RDS Proxy assumes responsibility for securely managing and using credentials to communicate with the backend database, standardizing the security model.

---

### 5. Conclusion

Mastering the physical limits of underlying OS database processes helps shape an accurate scaling strategy for Serverless applications. **Amazon RDS Proxy** completely resolves the connection bottleneck, establishing a foundation for building high-load, highly stable Serverless systems while ensuring the security and integrity of relational database infrastructure.

---

**Authors:** Thành Nhân, Nguyễn Cảnh Nguyên, Nguyễn Trọng Nhân, Nam Phan, Nguyễn Bá Nam

**References:**
- [Overview of connection management with Amazon RDS Proxy](https://aws.amazon.com/rds/proxy/)
- [How RDS Proxy works with AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/configuration-database.html)
- [Multiplexing and connection state management](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/rds-proxy.html)