Day 7 -- Microservice Decoupling & Asynchronous Explanation Pipeline
===================================================================

🎯 Goal
-------

Refactor the system architecture to **decouple explanation generation** from detection logic by:

-   moving explanation logic into a dedicated service

-   introducing Kafka-based asynchronous communication

-   enabling independent scaling and evolution of services

* * * * *

✅ What I Worked On
------------------

### 1\. Introduced New Kafka Topic

Created a new topic:

```
explanation-requests

```

Purpose:

-   act as the communication channel between:

    -   `detection-service` (producer)

    -   `explanation-service` (consumer)

* * * * *

### 2\. Defined Explanation Request Event

Created `ExplanationRequestEvent` in both services:

-   Fields:

    -   `transactionId`

    -   `matchedRules`

Design decision:

-   kept payload minimal

-   avoided sending full transaction data

-   ensured loose coupling between services

* * * * *

### 3\. Implemented Producer in detection-service

Created `ExplanationRequestPublisher`:

-   Publishes event after anomaly detection completes

-   Uses `KafkaTemplate`

-   Sends message to `explanation-requests` topic

* * * * *

### 4\. Refactored Detection Service

Updated `AnomalyDetectionService`:

-   Removed direct explanation generation

-   After saving `AnomalyResult`, now:

    -   builds `ExplanationRequestEvent`

    -   publishes it to Kafka

New flow:

```
Transaction → Detection → AnomalyResult → Kafka event

```

* * * * *

### 5\. Disabled Explanation Logic in detection-service

-   Stopped using:

    -   `ExplanationService`

    -   explanation persistence

-   Kept files for reference but removed them from execution flow

This completed logical decoupling.

* * * * *

### 6\. Built explanation-service Consumer Pipeline

In `explanation-service`:

#### a. Kafka Consumer

-   Implemented `@KafkaListener` for `explanation-requests`

-   Consumes events asynchronously

#### b. ExplanationService

-   Generates explanation based on `matchedRules`

-   Uses deterministic template logic

#### c. Persistence Layer

-   Created:

    -   `AnomalyExplanation` entity

    -   `AnomalyExplanationRepository`

-   Persists explanations into shared database

* * * * *

### 7\. End-to-End Validation

Performed full system test:

#### Input:

Kafka transaction events

#### Flow:

```
detection-service → publish explanation request
       ↓
Kafka
       ↓
explanation-service → consume → generate explanation → persist

```

#### Verified:

-   explanation-service receives events

-   explanations are correctly generated

-   data is persisted in `anomaly_explanations`

-   API returns correct results

* * * * *

### 8\. API Verification

Confirmed via `api-service`:

-   `/explanations/{transactionId}` returns:

    -   correct explanation text

    -   correct transaction mapping

    -   proper formatting

* * * * *

🧠 What I Learned
-----------------

### 1\. True Microservice Decoupling

-   Moving logic is not enough

-   Services must communicate via events

-   Kafka enables asynchronous, loosely coupled architecture

* * * * *

### 2\. Separation of Responsibilities

-   detection-service:

    -   decision-making (core logic)

-   explanation-service:

    -   interpretation (non-critical path)

This separation aligns with real-world system design.

* * * * *

### 3\. Asynchronous Processing Benefits

-   explanation generation no longer blocks detection

-   improves system throughput

-   allows different SLAs per service

* * * * *

### 4\. Event-Driven Design

-   services communicate through events, not direct calls

-   promotes:

    -   scalability

    -   resilience

    -   flexibility

* * * * *

### 5\. Foundation for AI Integration

By isolating explanation logic:

-   system is now ready for:

    -   LLM integration

    -   prompt-based explanation

    -   future AI enhancements

* * * * *

⚠️ Problems & Fixes
-------------------

### ❌ Tight coupling in previous design

-   Cause: explanation logic inside detection-service

-   Fix: extracted into separate service with Kafka

* * * * *

### ❌ Potential duplicate responsibility

-   Cause: explanation logic existed in two places

-   Fix: removed execution path from detection-service

* * * * *

🚀 Outcome
----------

At the end of Day 7:

-   explanation generation is fully decoupled

-   services communicate via Kafka events

-   system supports asynchronous processing

-   architecture is significantly closer to production-grade design

* * * * *

📌 System State Now
-------------------

```
transaction-events
    ↓
detection-service
    ├─ transaction persistence
    ├─ rule engine
    ├─ anomaly result persistence
    └─ publish explanation request
                ↓
        explanation-requests (Kafka)
                ↓
        explanation-service
            ├─ consume event
            ├─ generate explanation
            └─ persist explanation
                ↓
        api-service
            ├─ /transactions
            ├─ /anomaly-results
            └─ /explanations

```

* * * * *

🔜 Next Steps (Day 8)
---------------------

-   Integrate LLM for explanation generation

-   Replace template-based explanation with AI-generated text

-   Maintain rule engine as source of truth

-   Introduce prompt engineering and response validation

* * * * *

💡 Reflection
-------------

Day 7 marks the transition from a **functional microservice system** to a **well-architected distributed system**.\
By decoupling explanation logic and introducing event-driven communication, the system is now scalable, extensible, and ready for advanced AI integration.
