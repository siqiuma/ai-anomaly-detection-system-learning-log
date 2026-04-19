Day 12 -- Risk Visualization (UI Semantics)
==========================================

🎯 Goal
-------

Make anomaly risk immediately visible using visual cues instead of plain text.

* * * * *

✅ What I Worked On
------------------

### 1\. Built `RiskBadge` Component

-   Displays risk levels as colored badges:
    -   HIGH → red
    -   MEDIUM → orange
    -   LOW → green
    -   UNKNOWN → gray

* * * * *

### 2\. Integrated RiskBadge into UI

-   Applied to:
    -   Anomaly Results page
    -   Transaction Detail page

* * * * *

### 3\. Handled Data Normalization

-   Standardized risk values:
    -   `.trim()`
    -   `.toUpperCase()`
-   Prevented mismatch between backend and UI

* * * * *

### 4\. Added Styling

-   Designed pill-style badges
-   Used color to encode risk severity

* * * * *

🧠 What I Learned
-----------------

-   Visual encoding improves usability dramatically
-   Frontend must defensively handle inconsistent data
-   UI semantics (color, shape) are as important as data

* * * * *

🚀 Outcome
----------

-   Risk levels are instantly distinguishable
-   UI feels closer to a real monitoring system
