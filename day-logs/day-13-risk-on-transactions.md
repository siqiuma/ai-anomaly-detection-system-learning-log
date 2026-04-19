Day 13 -- Data Integration: Risk on Transactions Page
====================================================

🎯 Goal
-------

Bring anomaly insights directly into the main Transactions page.

* * * * *

✅ What I Worked On
------------------

### 1\. Combined Multiple Data Sources

-   Fetched:
    -   `/transactions`
    -   `/anomaly-results`
-   Joined data on `transactionId`

* * * * *

### 2\. Created Derived Data Model

-   Added fields to transactions:
    -   `riskLevel`
    -   `anomalyScore`

* * * * *

### 3\. Updated Table Columns

-   Added:
    -   Risk (with RiskBadge)
    -   Score

* * * * *

### 4\. Implemented Efficient Mapping

-   Used `Map` for O(1) lookup of anomaly results
-   Used `useMemo` for performance optimization

* * * * *

🧠 What I Learned
-----------------

-   Frontend can act as a lightweight data integration layer
-   Efficient data structures matter even in UI
-   Derived state improves user experience

* * * * *

🚀 Outcome
----------

-   Transactions page now shows risk at a glance
-   Eliminates need to drill down for basic insight
-   UI becomes a true operational dashboard

* * * * *
