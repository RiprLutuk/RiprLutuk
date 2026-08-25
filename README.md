<div align="center">

<!-- Animated Cyberpunk Mission Control SVG Banner -->
<a href="https://riprlutuk.github.io">
  <img src="https://raw.githubusercontent.com/RiprLutuk/RiprLutuk/main/assets/cyber-mission-control.svg" alt="Heri Riski Anto - The One-Man IT Division Architect" width="100%" />
</a>

<br><br>

[![Interactive Mission Control](https://img.shields.io/badge/🌐_ENTER_MISSION_CONTROL_PORTFOLIO-riprlutuk.github.io-f97316?style=for-the-badge&logo=google-chrome&logoColor=white)](https://riprlutuk.github.io)
[![Telegram Direct Dispatch](https://img.shields.io/badge/💬_DIRECT_TELEGRAM_DISPATCH-@riprlutuk-0088cc?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/riprlutuk)
[![LinkedIn Verified](https://img.shields.io/badge/💼_LINKEDIN_PROFILE-0a66c2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/riprlutuk)
[![Email Contact](https://img.shields.io/badge/✉️_DIRECT_EMAIL-rizqy.pra85%40gmail.com-ea4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:rizqy.pra85@gmail.com)

</div>

---

## 🏛️ Real-Time Production Architecture Pipeline

<div align="center">
  <img src="https://raw.githubusercontent.com/RiprLutuk/RiprLutuk/main/assets/architecture-radar.svg" alt="Real-Time Production Data Pipeline" width="100%" />
</div>

---

## ⚔️ SRE & DBA Incident Playbooks (Real-World War Room Drills)

<details open>
<summary><strong>🚨 Incident Drill #01: Zero-Downtime Heterogeneous Migration (Oracle 19c &rarr; PostgreSQL 16)</strong></summary>

```yaml
Organization: PT Link Net Tbk (Enterprise Telecom)
Scenario: Migrating multi-terabyte billing ledger from Oracle 19c to PostgreSQL 16 without taking downtime.
Heri Riski Anto Execution:
  1. Automated PL/SQL to PL/pgSQL translation of stored procedures, views, and sequence triggers.
  2. Dual-write Change Data Capture (Debezium WAL + Kafka) syncing incremental delta records with 0 lag.
  3. Continuous SHA-256 hash checksum validation on row blocks across both engines.
  4. Instant DNS/VIP cutover window: < 10 seconds with zero transaction loss.
Result: 100% data parity achieved, reducing annual database licensing overhead by 68%.
```
</details>

<details>
<summary><strong>🚨 Incident Drill #02: Preventing Connection Pool Starvation under 15,000 Worker Burst</strong></summary>

```yaml
Scenario: Microservice fleet experiences flash traffic spike, opening 15,000 concurrent DB connections (max_connections = 500 exhausted -> 503 errors).
Naive Approach: Increasing max_connections = 20,000 causes OS context switching thrashing & OOM crash.
Heri Riski Anto Solution:
  1. Deployed PgBouncer in transaction pooling mode directly in front of the primary cluster.
  2. Multiplexed 15,000 incoming client connections into 50 persistent server-side connections.
  3. Set pool_mode = transaction with query-level prepared statement recycling.
Result: Database CPU dropped from 98% to 14%, query response latency stabilized at 1.8ms.
```
</details>

<details>
<summary><strong>🚨 Incident Drill #03: Sub-Second Analytics on 50,000,000 Rows via ClickHouse Columnar OLAP</strong></summary>

```sql
-- PROBLEM: Traditional OLTP query aggregating monthly financial ledger takes 4,280ms
SELECT tenant_id, status, SUM(amount), AVG(processing_time)
FROM ledger_records_50m 
WHERE event_date >= '2025-01-01'
GROUP BY tenant_id, status;

-- CLICKHOUSE VECTORIZED ENGINE OPTIMIZATION:
-- ReplacingMergeTree engine with LowCardinality dictionary encoding & SIMD vector execution
CREATE TABLE ledger_analytics_ch (
    event_date Date,
    tenant_id LowCardinality(String),
    status LowCardinality(String),
    amount Decimal64(4),
    processing_time UInt32
) ENGINE = ReplacingMergeTree()
ORDER BY (tenant_id, event_date, status);

-- EXECUTION BENCHMARK:
-- Scanned 50,000,000 rows in 0.024s (Throughput: 33.3 GB/s)
-- Speedup: 178x faster than traditional row-oriented OLTP
```
</details>

<details>
<summary><strong>🚨 Incident Drill #04: Anti-Mock GPS Spoof Filter & Face Biometrics Verification</strong></summary>

```json
{
  "system": "PasPapan Enterprise Mobile Operations",
  "runtime": "Android Native (Java) & Flutter Engine",
  "security_layers": {
    "layer_1_kernel_gps": "ALLOW_MOCK_LOCATION check + FusedLocationProvider integrity: PASSED",
    "layer_2_face_biometrics": "Live Blink & Liveness Detection (Confidence: 99.4%): VERIFIED",
    "layer_3_network_token": "Signed JWT with Hardware Keystore Fingerprint: VALID"
  },
  "attendance_latency": "0.41s confirmed to cloud backend",
  "spoof_detection_rate": "100.00% Spoof Blocked"
}
```
</details>

---

## 🧭 Production Engineering Decision Matrix (Tech Radar)

Why I select specific technologies and architectures for high-stakes enterprise systems:

| Architectural Need | Standard Industry Choice | Heri Riski Anto Production Choice | Why This Choice Wins in Production |
| :--- | :--- | :--- | :--- |
| **Multi-DB API Access** | Linked Servers / Direct SQL Connections | **`DDAG` (Zero-Trust Go Gateway)** | Eliminates credential leakage, enables JWT token auth, and enforces query governance with < 2.5ms overhead. |
| **Real-Time Analytics** | OLTP Read-Replicas with Heavy Indexes | **`ch-olap-pipeline` (Kafka + ClickHouse)** | Decouples analytics from transaction workloads; columnar SIMD engine processes 50M rows in 0.024s. |
| **Offline-First Web Apps**| LocalStorage / IndexedDB Key-Value | **`WargaHub` (PGlite WASM Embedded DB)** | Runs a full relational PostgreSQL instance directly in the browser via WebAssembly with zero network dependency. |
| **Enterprise HRIS** | Standard Web Form Attendance | **`PasPapan` (Anti-Mock GPS + Face ID)** | Prevents fake GPS apps and photo spoofing with kernel-level checks and sub-second biometric verification. |
| **Database Migrations** | Static Dump & Restore with Downtime | **Continuous CDC Replication + Dual-Write** | Achieves zero data loss (RPO = 0) and reduces cutover maintenance windows to under 10 seconds. |

---

## 🚀 Flagship Production Platforms & Repositories

| Project & Dialect | Core Architecture & Highlights | Production Impact |
| :--- | :--- | :--- |
| **[⚡ DDAG](https://github.com/RiprLutuk/DDAG)**<br>`Go` `Postgres` `MSSQL` `Oracle` | Zero-Trust Dynamic SQL-to-REST API Gateway. Dynamically exposes multi-engine databases with JWT auth and PgBouncer connection pooling. | **< 2.5ms Latency Overhead**<br>Eliminates linked-server vulnerabilities |
| **[🔄 ch-olap-pipeline](https://github.com/RiprLutuk/ch-olap-pipeline)**<br>`Debezium` `Kafka` `ClickHouse` | Universal Real-Time CDC streaming pipeline capturing OLTP mutation logs into ClickHouse columnar storage for real-time analytics. | **33.3 GB/s Ingestion**<br>Sub-second reporting over 50M+ rows |
| **[🏛️ OpenOrg](https://github.com/RiprLutuk/openorg)**<br>`TypeScript` `Headless CMS` `KTA` | Single-tenant organization governance platform with digital membership ID (KTA), SKP credentialing, and cryptographic SHA-256 certificate validation. | **Tamper-Resistant**<br>Institutional governance hierarchy |
| **[🏡 WargaHub](https://github.com/RiprLutuk/WargaHub)**<br>`Vue 3 PWA` `Fastify` `PGlite WASM` | Civic governance, transparent budget ledger, and digital musyawarah consensus powered by embedded offline-capable WASM PostgreSQL. | **0.4ms WASM Queries**<br>100% Offline PWA ledger sync |
| **[📍 PasPapan](https://github.com/RiprLutuk/PasPapan)**<br>`Laravel 11` `Livewire` `Face ID` | Enterprise workforce operations with Anti-Mock GPS spoof detection, Face ID biometric attendance, and automated 1-click payroll calculation. | **0.00% Spoof Detected**<br>Multi-branch payroll automation |
| **[🔄 pg2ora-cdc](https://github.com/RiprLutuk/pg2ora_debezium_kafka)**<br>`Debezium` `Postgres` `Oracle 19c` | High-availability CDC integration synchronizing real-time financial ledger mutations from PostgreSQL to Oracle Database 19c with DLQ replay. | **0-Lag Sync**<br>Continuous WAL-to-Redo replication |

---

## 🌐 Live Diagnostic Lab & Interactive Sandbox on Website

Explore live simulations and tools on **[riprlutuk.github.io](https://riprlutuk.github.io)**:

* 🔬 **[DBA Diagnostic Lab](https://riprlutuk.github.io/#dba-lab):** Run real EXPLAIN ANALYZE comparison drills across 10M rows and trigger automated SRE failover scenarios.
* 💰 **[Cloud ROI Calculator](https://riprlutuk.github.io/#roi-calculator):** Estimate infrastructure savings with instant 1-click presets (Enterprise Migration, Fintech Scaling, 0-to-1 MVP).
* 🎯 **[Recruiter Persona Switcher](https://riprlutuk.github.io):** Toggle tailored mission briefings for Tech Recruiters, Startup Founders, and Enterprise VPs.
* 💻 **[Cyber CLI Terminal](https://riprlutuk.github.io):** Press `Ctrl+K` on any page to open the interactive UNIX CLI shell (`neofetch`, `psql`, `migrations`, `mobile`, `ats`).

---

<div align="center">

```
"Architecting resilient systems that scale silently, operate securely, and never lose a single byte of data."
```

**[Telegram (@riprlutuk)](https://t.me/riprlutuk)** • **[LinkedIn](https://linkedin.com/in/riprlutuk)** • **[Email](mailto:rizqy.pra85@gmail.com)** • **[Interactive Portfolio](https://riprlutuk.github.io)**

</div>
