Day 1 -- Project Setup & Environment Initialization
==================================================

🎯 Goal
-------

Set up the foundational project structure and ensure all core services and infrastructure can run successfully.

* * * * *

✅ What I Worked On
------------------

### 1\. Project Structure Setup

-   Created root directory: `ai-anomaly-detection-system`

-   Initialized three backend services using Spring Boot:

    -   `detection-service`

    -   `explanation-service`

    -   `api-service`

-   Created frontend using React + Vite:

    -   `web-ui`

* * * * *

### 2\. Environment Setup (Critical Step)

-   Fixed Node.js version issue:

    -   Switched from Node 16 → Node 20 using `nvm`

-   Resolved Java version conflict:

    -   Identified mismatch between `JAVA_HOME` and actual runtime

    -   Standardized environment to **Java 21 (Temurin)**

    -   Fixed shell initialization issue:

        -   Ensured `~/.zshrc` is sourced correctly

        -   Removed conflicting Java 13 config from `~/.bash_profile`

* * * * *

### 3\. Docker Infrastructure Setup

-   Created `docker-compose.yml`

-   Successfully launched:

    -   PostgreSQL

    -   Kafka

    -   Zookeeper

Verified via:

```
docker ps

```

* * * * *

### 4\. Service Configuration

-   Added `application.yml` for:

    -   `detection-service`

    -   `explanation-service`

    -   `api-service`

-   Configured:

    -   PostgreSQL datasource

    -   Kafka (for detection & explanation services)

    -   Unique ports:

        -   8081 (detection)

        -   8082 (explanation)

        -   8083 (api)

* * * * *

### 5\. Service Startup

-   Successfully started all services:

    -   detection-service

    -   explanation-service

    -   api-service

-   Verified all services are running (no startup errors)

* * * * *

### 6\. Frontend Setup

-   Created React app using Vite

-   Installed dependencies

-   Started dev server:

    -   Accessible at `http://localhost:5173`

* * * * *

🧠 What I Learned
-----------------

### 1\. Environment Consistency is Critical

-   Maven uses the Java version from `JAVA_HOME`, not just `java -version`

-   Multiple JDK installations can cause hidden conflicts

-   Shell initialization order (`.zshrc`, `.zprofile`, `.bash_profile`) matters

* * * * *

### 2\. Clean Development Environment Matters

-   Removed outdated Maven and Gradle PATH configurations

-   Avoided version conflicts by relying on:

    -   `mvnw` (Maven Wrapper)

    -   `nvm` (Node Version Manager)

* * * * *

### 3\. Microservice Separation from Day One

-   Split system into:

    -   Processing service (detection)

    -   AI/explanation service

    -   API layer

-   Helps with:

    -   Clear responsibility boundaries

    -   Future scalability

    -   Better system design explanation in interviews

* * * * *

### 4\. Infrastructure Before Business Logic

-   Brought up Kafka and PostgreSQL before writing any logic

-   Ensures:

    -   Early validation of connectivity

    -   Avoids debugging infra issues later

* * * * *

### 5\. 404 Does Not Mean Failure

-   Spring Boot returning `Whitelabel Error Page` (404) is expected

-   Indicates:

    -   Application is running

    -   No endpoint defined yet

* * * * *

⚠️ Problems & Fixes
-------------------

### ❌ Node / npm incompatibility

-   Cause: Node 16 incompatible with npm 11

-   Fix: Upgraded Node using `nvm`

* * * * *

### ❌ Maven using wrong Java version

-   Cause: `JAVA_HOME` pointing to Java 13

-   Fix:

    -   Updated `.zshrc`

    -   Removed conflicting config from `.bash_profile`

    -   Verified using `./mvnw -v`

* * * * *

### ❌ Homebrew node linking issues

-   Cause: Permission conflict in `/usr/local/include/node`

-   Resolution:

    -   Switched to `nvm` instead of forcing Homebrew setup

* * * * *

🚀 Outcome
----------

At the end of Day 1:

-   All backend services are running

-   Frontend is running

-   Kafka and PostgreSQL are running

-   Development environment is stable and consistent

* * * * *

📌 Next Steps (Day 2)
---------------------

-   Create `Transaction` entity

-   Enable Hibernate table creation

-   Add repository layer

-   Insert first test data into PostgreSQL

-   Begin Kafka integration (first message flow)

* * * * *

💡 Reflection
-------------

Today was not about building features, but about building a **reliable foundation**.\
Fixing environment issues early prevents significant debugging overhead later and ensures smoother development moving forward.
