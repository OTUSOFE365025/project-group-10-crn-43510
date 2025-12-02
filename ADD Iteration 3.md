# ADD Iteration 3: Addressing Availability, Scalability, and Security

This section presents the results of the activities performed in the third iteration of the design process, focusing on the critical quality attributes identified in the ATAM assessment.

## Step 1: Review Inputs

The first step involves reviewing the inputs and identifying which requirements will be considered as drivers for this iteration. The inputs are summarized in the following table.

| Category | Details |
| :--- | :--- |
| **Design Purpose** | Refine the physical deployment and integration architecture to ensure the system meets critical quality standards for a cloud-native environment. |
| **Primary Drivers** | **Availability:** RS11 (99.5% uptime), RA6 (Failover), RM1 (Zero-downtime).<br>**Performance:** RS10 (2s latency), RA7 (5,000 users).<br>**Security:** RS7 (SSO), RS8 (Data privacy), RM7 (Role-based access). |
| **Constraints** | **R7:** The system must be deployable as a cloud-native, scalable service.<br>**RD2:** The system must use standard APIs (REST/GraphQL) for interoperability. |

## Step 2: Establish Iteration Goal by Selecting Drivers

The goal of this iteration is to satisfy the high-priority quality attribute scenarios related to performance, availability, and security. The following drivers are selected:

* **QA-1 (Performance):** The system shall respond to queries within 2 seconds (RS10) and scale to 5,000 users (RA7).
* **QA-2 (Availability):** The system shall remain available 99.5% of the time (RS11) with automatic fail-over (RA6).
* **QA-3 (Security):** The system shall enforce strict authentication via SSO (RS7), encrypt sensitive data (R8), and enforce role-based maintenance access (RM7).

## Step 3: Choose Elements of the System to Refine

To address these drivers, we refine the physical nodes and deployment units identified in previous iterations:
* Application Server (Conversation Orchestrator & Gateway)
* Data Storage Layer (Database & Cache)
* External Integration Interfaces (SSO, LMS, AI Service)
* Monitoring & Security Infrastructure

## Step 4: Choose Design Concepts That Satisfy the Selected Drivers

The following design concepts and tactics are selected to satisfy the drivers:

| Design Decision | Rationale |
| :--- | :--- |
| **Active Redundancy (Load Balanced Cluster)** | **(Availability & Scalability)** To meet RS11 and RA7, the Application Server is deployed as a cluster of stateless instances behind a Load Balancer. This allows traffic to be distributed (Performance) and rerouted if a node fails (Availability). |
| **Database Replication (Master-Slave)** | **(Availability)** To address RA6, the database uses Master-Slave replication. Writes go to the Master; reads are distributed to replicas. If the Master fails, a replica is promoted to ensure continuity. |
| **API Gateway with OAuth2 & RBAC** | **(Security)** An API Gateway serves as the single entry point. It integrates with the IAM service to validate SSO tokens (RS7) and enforces Role-Based Access Control (RBAC) to ensure only authorized users (e.g., Maintainers vs. Students) access specific endpoints (RM7, RS8). |
| **Encryption (TLS & AES)** | **(Security/Privacy)** To meet R8 and RA5, all data in transit is encrypted via TLS 1.3. Sensitive data at rest (grades, logs) is encrypted using AES-256, ensuring compliance with institutional privacy policies. |
| **Caching Strategy (Redis)** | **(Performance)** A distributed cache stores frequent query responses. This reduces the processing load on the AI and Database layers, ensuring the 2-second response time (RS10). |

## Step 5: Instantiate Architectural Elements and Define Interfaces

The instantiation design decisions are summarized below:

| Element | Responsibility | Key Interfaces |
| :--- | :--- | :--- |
| **Load Balancer** | Distributes traffic to healthy instances; performs health checks. | `routeRequest(HttpRequest)` |
| **API Gateway** | Enforces security policies (RS8); terminates SSL; performs rate limiting. | `forwardRequest(context)` |
| **Authorization Service / IAM** | Validates SSO tokens; enforces RBAC policies (e.g., checking if user has `Lecturer` or `Maintainer` role before allowing write operations) (RM7, RL5). | `validateToken(token)`<br>`authorize(user, resource, action)` |
| **AIDAP Backend Cluster** | Stateless containers running the core business logic and AI orchestration. | `processQuery(userContext, query)` |
| **Monitoring & Logging Service** | Aggregates health, latency, and error metrics; logs performance data for model accuracy (RM2, RM4). | `logMetric(service, metric, value)`<br>`alert(condition)` |
| **Cache Layer (Redis)** | Stores high-frequency data with Time-To-Live (TTL). | `get(key)`, `set(key, value, ttl)` |

## Step 6: Sketch Views (Deployment Diagram)

The diagram below illustrates the refined Deployment View, highlighting the redundancy (Clusters, Replicas) and security boundaries (Gateway).

<img width="880" height="785" alt="image" src="https://github.com/user-attachments/assets/d279bc6a-d162-4931-b6b5-2f53b3b1a1f7" />


## Step 7: Analyze Design and Review Iteration

The following table summarizes the status of the drivers following this iteration.

| Not Addressed | Partially Addressed | Completely Addressed | Design Decisions Made |
| :--- | :--- | :--- | :--- |
| | | **Performance (RS10, RA7)** | addressed via Load Balancing, Caching, and Horizontal Scaling. |
| | | **Availability (RS11, RA6)** | addressed via Active Redundancy and Database Replication. |
| | | **Security (RS7, RS8, RM7)** | addressed via API Gateway, SSO Integration, Encryption, and RBAC. |
| | **Operations (RM1-RM4)** | | Monitoring components instantiated; specific dashboards and pipeline tools pending selection. |
