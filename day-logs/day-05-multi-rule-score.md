Day 5 -- Multi-Rule Scoring & Risk Tiering
=========================================

🎯 Goal
-------

Upgrade the anomaly detection system from **single-rule evaluation** to a more realistic **multi-rule scoring system**, including:

-   multiple rules

-   score aggregation

-   risk tier classification (LOW / MEDIUM / HIGH)

* * * * *

✅ What I Worked On
------------------

### 1\. Introduced Second Detection Rule

Implemented `SuspiciousCountryRule`:

-   Logic:

    -   If `country ∈ {RU, IR, KP}` → score +30

-   Registered as Spring component (`@Component`)

-   Automatically included in rule engine via dependency injection

* * * * *

### 2\. Upgraded Rule Engine for Multi-Rule Evaluation

Refactored `DetectionRuleEngine` to:

-   Evaluate all rules

-   Aggregate:

    -   total score

    -   list of matched rules

Changed from:

```
Single rule → single result

```

to:

```
Multiple rules → aggregated result

```

* * * * *

### 3\. Enhanced Rule Engine Result Model

Updated `RuleEngineResult`:

-   Replaced single `matchedRule` with:

    -   `List<String> matchedRules`

-   Enabled tracking of multiple rule triggers

* * * * *

### 4\. Schema Evolution (Important Real-World Step)

Updated database schema:

-   Renamed column:

    ```
    matched_rule → matched_rules

    ```

-   Refactored:

    -   `AnomalyResult` entity (detection-service)

    -   `AnomalyResult` entity (api-service)

Handled schema change manually using SQL migration approach.

* * * * *

### 5\. Refactored Detection Service

Updated `AnomalyDetectionService`:

-   Consumes aggregated rule engine result

-   Computes final risk level based on total score:

| Score Range | Risk Level | Flagged |
| --- | --- | --- |
| 0 | LOW | false |
| 1--79 | MEDIUM | true |
| 80+ | HIGH | true |

-   Converts matched rules list into comma-separated string

* * * * *

### 6\. End-to-End Multi-Rule Validation

Tested 4 scenarios via Kafka:

| Case | Input | Expected | Result |
| --- | --- | --- | --- |
| Country only | RU + low amount | MEDIUM (30) | ✅ |
| Amount only | 1500 USD | HIGH (80) | ✅ |
| Both rules | RU + 2000 USD | HIGH (110) | ✅ |
| No rule | US + low amount | LOW (0) | ✅ |

* * * * *

### 7\. API Verification

Confirmed:

-   `/anomaly-results` returns correct aggregated results

-   `/anomaly-results/{transactionId}` reflects:

    -   score

    -   risk level

    -   multiple matched rules

* * * * *

🧠 What I Learned
-----------------

### 1\. Multi-Rule Aggregation is Core to Real Systems

-   Real anomaly detection systems rely on **score accumulation**, not binary decisions

-   Multiple weak signals combine into strong risk indicators

* * * * *

### 2\. Rule Engine Design Shows Its Value

-   Adding a new rule required:

    -   creating a new class

    -   no changes to engine logic

-   Confirms extensibility of architecture

* * * * *

### 3\. Schema Evolution is Inevitable

-   Moving from single → multiple rules required DB change

-   Learned:

    -   backward compatibility considerations

    -   need for migration strategy in real systems

* * * * *

### 4\. Risk is a Spectrum, Not Binary

-   Introduced MEDIUM tier

-   System now models:

    -   low-risk noise

    -   moderate anomalies

    -   high-risk events

* * * * *

### 5\. Separation of Responsibilities Remains Clean

-   Rule logic → `DetectionRule`

-   Aggregation → `DetectionRuleEngine`

-   Risk mapping → `AnomalyDetectionService`

-   Persistence → repository layer

* * * * *

⚠️ Problems & Fixes
-------------------

### ❌ Single matched rule limitation

-   Issue: could not represent multiple rule triggers

-   Fix: upgraded to list-based aggregation

* * * * *

### ❌ Schema mismatch after code change

-   Issue: entity no longer matched DB

-   Fix: manual SQL migration (`ALTER TABLE`)

* * * * *

🚀 Outcome
----------

At the end of Day 5:

-   System supports multi-rule anomaly detection

-   Scores are aggregated across rules

-   Risk classification is more realistic

-   Detection results reflect combined signals

-   API fully exposes enhanced results

* * * * *

📌 System State Now
-------------------

```
Kafka → detection-service
      → Transaction persistence
      → Rule Engine
          → LargeAmountRule (+80)
          → SuspiciousCountryRule (+30)
      → Score aggregation
      → Risk classification (LOW / MEDIUM / HIGH)
      → AnomalyResult persistence
      ↓
api-service → REST API

```

* * * * *

🔜 Next Steps (Day 6)
---------------------

-   Introduce anomaly explanation layer

-   Persist explanation results

-   Link explanations to anomaly results

-   Prepare for AI/LLM-based explanation generation

* * * * *

💡 Reflection
-------------

Day 5 marks the transition from a simple rule-based system to a **scoring-based detection engine**.\
The system now better reflects how real-world fraud and anomaly detection platforms operate---by combining multiple weak signals into meaningful risk assessments.
