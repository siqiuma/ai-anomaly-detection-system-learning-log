Day 3 -- Anomaly Detection & Result Pipeline
===========================================

🎯 Goal
-------

Extend the system from **data ingestion** to **actual anomaly detection** by:

-   Evaluating transactions using rules

-   Persisting anomaly results

-   Exposing detection results via API

* * * * *

✅ What I Worked On
------------------

### 1\. Anomaly Result Data Model

-   Created `AnomalyResult` entity in `detection-service`

-   Mapped to new table: `anomaly_results`

-   Fields include:

    -   `transactionId`

    -   `anomalyScore`

    -   `riskLevel`

    -   `flagged`

    -   `matchedRule`

    -   `detectedAt`

-   Verified Hibernate auto-created the table

* * * * *

### 2\. Repository Layer

-   Implemented `AnomalyResultRepository`

-   Added query method:

    -   `findByTransactionId`

* * * * *

### 3\. Detection Logic (First Rule 🚀)

Implemented first rule:

> **Large Amount Rule**\
> If `amount >= 1000` → flagged as HIGH risk

Created `AnomalyDetectionService`:

-   Evaluates transaction

-   Assigns:

    -   score

    -   risk level

    -   rule name

-   Persists result

* * * * *

### 4\. Integrated Detection into Ingestion Flow

Updated `TransactionIngestionService`:

```
Kafka event → save transaction → evaluate → save anomaly result

```

Key behaviors:

-   Ensures idempotency on transaction insert

-   Immediately triggers detection after persistence

* * * * *

### 5\. End-to-End Validation (Kafka → Detection → DB)

Sent test events:

-   High amount transaction (`txn_300001`)

-   Low amount transaction (`txn_300002`)

Verified:

-   Kafka consumer processed events

-   Transactions persisted

-   Anomaly results generated correctly

* * * * *

### 6\. API Exposure (api-service)

Added full read support for anomaly results:

-   `AnomalyResult` entity (read model)

-   `AnomalyResultRepository`

-   `AnomalyResultQueryService`

-   `AnomalyResultController`

Endpoints:

-   `GET /anomaly-results`

-   `GET /anomaly-results/{transactionId}`

* * * * *

### 7\. API Verification

Confirmed:

-   API returns anomaly results correctly

-   Data matches database content

-   Both HIGH and LOW risk cases are visible

* * * * *

🧠 What I Learned
-----------------

### 1\. Detection is a Separate Domain Layer

-   Transaction ingestion ≠ anomaly detection

-   Detection produces its own dataset (`anomaly_results`)

* * * * *

### 2\. Event-Driven Processing + Synchronous Evaluation

-   Detection is triggered immediately after ingestion

-   This is a practical V1 design for real-time systems

* * * * *

### 3\. Rule Design Should Start Simple

-   Began with deterministic rule (`amount >= 1000`)

-   Avoided premature complexity (multi-rule engine, ML, etc.)

* * * * *

### 4\. System Now Has Multiple Data Views

The system now maintains:

-   Raw transactions

-   Detection results

This is an early form of **CQRS-style separation**

* * * * *

### 5\. Microservice Responsibility Clarity

-   detection-service:

    -   ingestion

    -   persistence

    -   detection logic

-   api-service:

    -   read-only query layer

* * * * *

⚠️ Problems & Fixes
-------------------

### ❌ Missing anomaly table

-   Cause: entity not yet created

-   Fix: added `AnomalyResult` entity with `ddl-auto=update`

* * * * *

### ❌ Detection not triggered

-   Cause: ingestion flow not wired to detection service

-   Fix: integrated detection call after save

* * * * *

### ❌ API not returning results initially

-   Cause: api-service not started

-   Fix: started service and verified endpoints

* * * * *

🚀 Outcome
----------

At the end of Day 3:

-   System performs real anomaly detection

-   Detection results are persisted and queryable

-   Kafka-driven ingestion is fully integrated with detection logic

* * * * *

📌 System State Now
-------------------

```
Kafka → detection-service
      → transactions table
      → anomaly_results table
      ↓
api-service → REST API

```

* * * * *

🔜 Next Steps (Day 4)
---------------------

-   Introduce rule engine abstraction

-   Support multiple rules

-   Aggregate anomaly scores

-   Improve detection extensibility

* * * * *

💡 Reflection
-------------

Day 3 is the point where the system gained real business meaning.\
Instead of just moving data, it now **interprets and evaluates it**, marking the transition from infrastructure setup to domain-driven functionality.
