# SD_24BCS10450_AYUSH-BANSAL

## Experiment 1 – URL Shortener (TinyURL) System Design

### Student Information

* **Name:** Ayush Bansal
* **Roll Number:** 24BCS10450
* **Course:** System Design
* **Experiment:** URL Shortener System Architecture

---

## Project Overview

This experiment demonstrates the **system design of a scalable URL Shortener (TinyURL-like service)**. The objective is to design a distributed system capable of shortening long URLs, redirecting users efficiently, storing mappings reliably, and collecting click analytics for monitoring and reporting.

The design focuses on **high availability, scalability, performance, and security** while supporting millions of users and billions of URL records.

---

## Functional Requirements

* Shorten a long URL into a unique short URL.
* Redirect users from the short URL to the original URL.
* Ensure every generated short URL is unique.
* Support URL expiration.
* Support user login and signup for managing URLs.

---

## Non-Functional Requirements

* **High Availability** – service should remain accessible during failures.
* **Scalability** – capable of storing billions of URLs.
* **Performance** – low latency for URL redirection.
* **Reliability** – no duplicate URL mappings.
* **Security** – prevent malicious or invalid URLs.

---

## System Architecture

The proposed architecture includes:

* **Client (Web / Mobile App)**
* **DNS**
* **Load Balancer**
* **Application Servers**
* **Unique ID / Short Code Generator**
* **Cache Layer (Redis)**
* **Primary Database (Sharded by short code hash)**
* **Read Replicas**
* **Message Queue (Kafka)**
* **Analytics Service**
* **Click Analytics Database**
* **CDN for popular redirects**

This architecture separates **read-heavy redirect traffic** from **analytics processing**, improving overall system throughput.

---

## Database Schema

### USERS

| Field      | Type         |
| ---------- | ------------ |
| user_id    | BIGINT       |
| name       | VARCHAR(100) |
| email      | VARCHAR(100) |
| created_at | TIMESTAMP    |

### URL_MAPPING

| Field       | Type         |
| ----------- | ------------ |
| id          | BIGINT       |
| short_code  | VARCHAR(7)   |
| long_url    | VARCHAR(500) |
| user_id     | BIGINT       |
| created_at  | TIMESTAMP    |
| expiry_date | TIMESTAMP    |
| metadata    | JSONB        |
| is_active   | BOOLEAN      |

### CLICK_ANALYTICS

| Field       | Type         |
| ----------- | ------------ |
| id          | BIGINT       |
| short_code  | VARCHAR(7)   |
| clicked_at  | TIMESTAMP    |
| ip_address  | VARCHAR(45)  |
| device_type | VARCHAR(50)  |
| location    | VARCHAR(100) |

---

## API Design

### Create Short URL

```http
POST /api/v1/urls
```

### Redirect to Original URL

```http
GET /{short_code}
```

### Get URL Details and Analytics

```http
GET /api/v1/urls/{short_code}
```

### Update Expiry or Metadata

```http
PATCH /api/v1/urls/{short_code}
```

### Delete or Deactivate URL

```http
DELETE /api/v1/urls/{short_code}
```

---

## Capacity Estimation

### Assumptions

* Daily Users: **1 Million**
* Requests per Second: **~12 RPS average**
* Peak Redirect Traffic: **~1200 RPS**
* Peak URL Creation Traffic: **~250 RPS**
* Storage Duration: **10 Years**

### Storage Estimation

* Average record size: **~600 Bytes**
* Daily storage: **~600 MB**
* Yearly storage: **~219 GB**
* 10-year storage: **~2.2 TB**

### Cache Estimation

* 20% of URLs generate 80% of traffic.
* Hot URLs cached in **Redis** require approximately **12–16 GB RAM**.

---

## Project Structure

```
SD_24BCS10450_AYUSH-BANSAL
│
├── README.md
├── Experiment_1.drawio
└── architecture.png
```

---

## Technologies Used

* **draw.io** – architecture and UML-style diagrams
* **Git & GitHub** – version control and repository hosting
* **Markdown** – project documentation

---

## Learning Outcomes

Through this experiment, the following concepts were explored:

* Distributed system architecture
* Load balancing strategies
* Database sharding and replication
* Redis caching for high-performance reads
* Asynchronous event processing with message queues
* REST API design principles
* Storage and traffic capacity estimation

---

## Conclusion

This experiment presents a **scalable and production-oriented TinyURL system design** capable of handling high redirect traffic while maintaining reliability, low latency, and efficient analytics processing. The architecture demonstrates how modern distributed systems use **caching, sharding, replication, and asynchronous processing** to achieve scalability and fault tolerance.

---

## Repository

GitHub Repository:

**https://github.com/bansalayush475/SD_24BCS10450_AYUSH-BANSAL**

---

### Author

**Ayush Bansal**
24BCS10450
Chandigarh University
