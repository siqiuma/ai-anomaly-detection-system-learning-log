Day 16 -- Write Path: Publish Transaction via API
================================================

🎯 Goal
-------

Enable UI-driven transaction creation by introducing a **write API** that publishes events to Kafka.

* * * * *

✅ What I Worked On
------------------

### 1\. Added Kafka Producer to API Service

-   Configured Kafka producer in `api-service`
-   Enabled publishing to `transaction-events` topic

* * * * *

### 2\. Created Publish Endpoint

```
POST /transactions/publish
```

-   Accepts transaction payload
-   Converts to `TransactionEvent`
-   Publishes to Kafka

* * * * *

### 3\. Introduced Request DTO

-   `PublishTransactionRequest`
-   Defines input contract for UI

* * * * *

### 4\. Built Publish Service

-   `TransactionPublishService`
-   Handles event creation and Kafka publishing

* * * * *

### 5\. Verified End-to-End Flow

Tested:

-   API → Kafka → detection-service → DB → explanation-service → DB

* * * * *

🧠 What I Learned
-----------------

-   Separation of write path vs read path
-   Event-driven ingestion pattern
-   Importance of decoupling producers from consumers

* * * * *

🚀 Outcome
----------

-   System now supports **external input via API**
-   Enables real-time demo and interaction
