Day 4 -- Rule Engine Refactor & Extensible Detection Design
==========================================================

🎯 Goal
-------

Refactor anomaly detection logic from a hardcoded implementation into a **scalable and extensible rule engine architecture**.

* * * * *

✅ What I Worked On
------------------

### 1\. Refactored Detection Logic into Rule-Based Design

Replaced previous hardcoded logic:

```
if (amount >= 1000) { ... }

```

with a **rule-driven architecture**, enabling:

-   modular rule definition

-   easier extensibility

-   cleaner separation of concerns

* * * * *

### 2\. Introduced Rule Interface

Created `DetectionRule` interface:

-   Defines:

    -   `getName()`

    -   `evaluate(Transaction)`

This establishes a contract for all future detection rules.

* * * * *

### 3\. Created Rule Evaluation Result Model

Implemented `RuleEvaluationResult`:

-   Contains:

    -   rule name

    -   score contribution

    -   trigger flag

This separates:

-   rule logic

-   scoring logic

* * * * *

### 4\. Implemented First Rule as Component

Created `LargeAmountRule`:

-   Logic:

    -   `amount >= 1000 → score = 80`

-   Annotated with `@Component` to enable automatic discovery

* * * * *

### 5\. Built Rule Engine (Core Design 🔥)

Created `DetectionRuleEngine`:

-   Injects all rules via:

    ```
    List<DetectionRule>

    ```

-   Iterates through rules and aggregates:

    -   total score

    -   matched rule

This allows:

-   plug-and-play rule addition

-   no modification to engine when adding rules

* * * * *

### 6\. Introduced Rule Engine Result Model

Created `RuleEngineResult`:

-   Encapsulates:

    -   aggregated score

    -   matched rule

* * * * *

### 7\. Refactored Detection Service

Updated `AnomalyDetectionService` to:

-   call rule engine instead of using hardcoded logic

-   compute:

    -   anomaly score

    -   risk level

    -   flagged status

-   persist `AnomalyResult`

* * * * *

### 8\. End-to-End Validation

Tested using Kafka events:

-   High amount transaction (`txn_400001`)

-   Low amount transaction (`txn_400002`)

Verified:

-   Rule engine correctly applied logic

-   Scores computed as expected:

    -   HIGH → 80

    -   LOW → 0

-   Results persisted and queryable via API

* * * * *

🧠 What I Learned
-----------------

### 1\. Rule Engine Pattern

-   Encapsulates business rules as independent units

-   Improves maintainability and extensibility

-   Avoids monolithic detection logic

* * * * *

### 2\. Dependency Injection for Extensibility

-   Spring automatically injects all rule implementations

-   New rules can be added without modifying existing code

* * * * *

### 3\. Separation of Concerns

-   Rule logic (DetectionRule)

-   Aggregation logic (RuleEngine)

-   Persistence logic (DetectionService)

Each layer now has a clear responsibility.

* * * * *

### 4\. Scoring Model Evolution

-   Moved from fixed scoring (10/80) to rule-based scoring

-   Adopted:

    -   "no rule triggered → score = 0"

-   More aligned with real-world risk systems

* * * * *

### 5\. Backward Compatibility Awareness

-   Observed difference between old and new scoring outputs

-   Learned importance of consistent scoring strategy

* * * * *

⚠️ Problems & Fixes
-------------------

### ❌ Hardcoded detection logic

-   Issue: not scalable for multiple rules

-   Fix: introduced rule abstraction + engine

* * * * *

### ❌ Tight coupling of logic and persistence

-   Issue: detection logic embedded directly in service

-   Fix: extracted into dedicated rule engine

* * * * *

🚀 Outcome
----------

At the end of Day 4:

-   System supports modular rule-based detection

-   New rules can be added without modifying core logic

-   Detection pipeline remains fully functional

-   Architecture is significantly more extensible and production-like

* * * * *

📌 System State Now
-------------------

```
Kafka → detection-service
      → Transaction persistence
      → Rule Engine
          → LargeAmountRule
      → AnomalyResult persistence
      ↓
api-service → REST API

```

* * * * *

🔜 Next Steps (Day 5)
---------------------

-   Add additional rules (e.g., suspicious country)

-   Support multi-rule scoring

-   Introduce risk tiers (LOW / MEDIUM / HIGH)

-   Improve rule aggregation logic

* * * * *

💡 Reflection
-------------

Day 4 marks a shift from functional correctness to **architectural quality**.\
The system is no longer just working --- it is now **designed for growth**, allowing new detection rules to be added with minimal effort.
