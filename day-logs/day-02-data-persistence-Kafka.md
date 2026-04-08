Day 2 -- Data Persistence & Kafka Ingestion Pipeline
===================================================

🎯 Goal
-------

Transform the system from a "running skeleton" into a **data-processing system** by:

-   Persisting real data into PostgreSQL

-   Exposing data via API

-   Ingesting data through Kafka (event-driven pipeline)

* * * * *

✅ What I Worked On
------------------

### 1\. Database Integration (JPA + Entity)

-   Created `Transaction` entity in `detection-service`

-   Configured Hibernate with `ddl-auto=update`

-   Verified automatic table creation in PostgreSQL (`transactions`)

* * * * *

### 2\. Repository Layer

-   Implemented `TransactionRepository`

-   Added method:

    -   `findByTransactionId` (used for idempotency)

* * * * *

### 3\. Initial Data Validation

-   Used `CommandLineRunner` (`DataInitializer`) to insert a test transaction

-   Verified:

    -   Data successfully written to DB

    -   Query works via SQL

* * * * *

### 4\. Read API (api-service)

-   Duplicated `Transaction` entity for read service

-   Created:

    -   `TransactionRepository`

    -   `TransactionQueryService`

    -   `TransactionController`

-   Implemented endpoints:

    -   `GET /transactions`

    -   `GET /transactions/{transactionId}`

-   Verified:

    -   API returns JSON data

    -   Data matches DB content

* * * * *

### 5\. Kafka Integration (Core Milestone 🚀)

#### Topic Setup

-   Created Kafka topic:

    -   `transaction-events`

* * * * *

#### Event Model

-   Created `TransactionEvent` (separate from entity)

-   Established separation:

    -   **Event model (Kafka)** vs **Entity model (DB)**

* * * * *

#### Consumer Implementation

-   Built `TransactionEventConsumer` using `@KafkaListener`

-   Introduced `TransactionIngestionService` to:

    -   handle mapping

    -   enforce idempotency

    -   persist data

* * * * *

#### Kafka Configuration Fixes

-   Added `spring-kafka` dependency

-   Replaced `kafka-streams` (incorrect for this use case)

-   Fixed JSON deserialization issues:

    -   Added `ErrorHandlingDeserializer`

    -   Configured:

        -   `spring.json.value.default.type`

        -   `spring.json.use.type.headers=false`

* * * * *

### 6\. End-to-End Flow Validation

#### Sent message via Kafka console:

```
{
  "transactionId":"txn_200001",
  "accountId":"acc_9001",
  "userId":"user_8001",
  "merchantId":"m_7001",
  "deviceId":"dev_6001",
  "amount":499.99,
  "currency":"USD",
  "country":"US",
  "paymentMethod":"DEBIT_CARD",
  "transactionType":"PURCHASE",
  "eventTime":"2026-04-07T20:10:00Z"
}

```

#### Verified:

-   Kafka consumer received event

-   Data persisted to PostgreSQL

-   API successfully returned new transaction

* * * * *

🧠 What I Learned
-----------------

### 1\. Entity vs Event Model Separation

-   Entity = database mapping

-   Event = message payload

-   Keeping them separate improves flexibility and system design clarity

* * * * *

### 2\. Idempotency Design

-   Used `transactionId` as business key

-   Prevented duplicate inserts

-   Introduced real-world ingestion safety pattern

* * * * *

### 3\. Kafka Deserialization Pitfalls

-   Console producer does not include type headers

-   Required:

    -   default type configuration

    -   disabling header-based type resolution

-   Learned importance of `ErrorHandlingDeserializer`

* * * * *

### 4\. Microservice Responsibility Separation

-   detection-service → writes data

-   api-service → reads data

This introduces early **CQRS-style separation**

* * * * *

### 5\. Debugging Approach

-   Narrowed issues layer-by-layer:

    -   infra → config → dependency → runtime behavior

-   Identified difference between:

    -   service startup success

    -   runtime message processing failure

* * * * *

⚠️ Problems & Fixes
-------------------

### ❌ Kafka dependency mismatch

-   Issue: used `kafka-streams` instead of `spring-kafka`

-   Fix: replaced dependency

* * * * *

### ❌ JSON deserialization failure

-   Issue: `SerializationException`

-   Fix:

    -   added `ErrorHandlingDeserializer`

    -   configured default value type

* * * * *

### ❌ API returned Whitelabel error

-   Cause: `api-service` not running

-   Fix: started correct service

* * * * *

🚀 Outcome
----------

At the end of Day 2:

-   System can persist transactions via JPA

-   API layer can query and return data

-   Kafka ingestion pipeline is fully functional

-   End-to-end event-driven data flow is verified

* * * * *

📌 System State Now
-------------------

```
Kafka → detection-service → PostgreSQL → api-service → HTTP

```

* * * * *

🔜 Next Steps (Day 3)
---------------------

-   Introduce `AnomalyResult` entity

-   Implement detection rules

-   Evaluate transactions on ingestion

-   Store anomaly results

-   Begin building explanation pipeline

* * * * *

💡 Reflection
-------------

Day 2 marks the transition from a static system to a **data-driven system**.\
The most important shift was moving from manual data insertion to **event-driven ingestion**, which aligns with real-world distributed system design.
