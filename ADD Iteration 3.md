# ADD Iteration 3 – AIDAP

## 1. Iteration goal and selected drivers

The goal of Iteration 3 is to refine the AIDAP architecture to satisfy key quality attributes for the most critical usage scenarios, focusing on performance, availability, and security for conversational queries, notifications, and integrations with university systems.

The following requirements are treated as drivers in this iteration:  
- Performance: RS10 (average 2‑second response time), R7 (cloud‑native scalable service), RD1–RD3 (synchronization, standard APIs, graceful failure handling), RM4 (logging performance metrics).  
- Availability: RS11 (99.5% monthly availability), RA6–RA7 (high availability, failover, scalability), RM1 (zero‑downtime updates), RM6 (backup and restore).  
- Security: R8 (privacy and policy compliance), RS7–RS8 (SSO and data isolation), RL8 (authorization on course data), RA2–RA5 (policies, security and privacy), RM7 (role‑based access).

## 2. Elements selected for refinement

Iterations 1 and 2 established the overall AIDAP structure, including client channels, a conversational back end, integration layer, and data stores.  
In Iteration 3, the following elements are refined to address the selected drivers:  
- Conversation/API Gateway and Channel Adapters (web, mobile, voice).  
- Conversation Orchestrator and AI Services that interpret queries and compose responses.  
- Integration Services for LMS, registration, calendar, and email systems.  
- Identity and Access Management (IAM) and Authorization components.  
- Data Stores for interaction history, configuration, analytics, and logs.

These elements are the main contributors to response time, uptime, and protection of sensitive institutional data and therefore are the focus of design decisions in this iteration.

## Step 3: Choose Elements of the System to Refine

In this iteration, the following elements identified in previous iterations are selected for refinement:
* Conversation/API Gateway
* Conversation Orchestrator & AI Services
* Integration Services (LMS, Registration, Calendar)
* Identity and Access Management (IAM)
* Data Stores (Interaction History, Configuration, Logs)

## Step 4: Choose Design Concepts That Satisfy the Selected Drivers

The following design concepts are selected to satisfy the drivers.

| Design Decision | Rationale |
| :--- | :--- |
| **Caching Strategy (Gateway & Orchestrator)** | **(Performance)** Frequently accessed data (course schedules, deadlines) are cached with configurable TTL. [cite_start]This reduces load on downstream systems and ensures the 2-second response time required by RS10[cite: 629]. |
| **Asynchronous Processing (Message Queue)** | **(Performance/Scalability)** Non-critical tasks (detailed logging, analytics aggregation) are offloaded to background workers via a message queue. [cite_start]This prevents blocking user requests, supporting RS10 and RM4[cite: 629, 635]. |
| **Active Redundancy & Load Balancing** | **(Availability)** Stateless services (Gateway, Orchestrator) run in multiple active instances behind a load balancer. [cite_start]Traffic is automatically redistributed if a node fails, satisfying RS11 and RA6[cite: 629, 633]. |
| **Resilient Integration (Circuit Breakers)** | **(Availability)** Calls to external data sources (LMS, Registration) are wrapped in retry and circuit-breaker logic. [cite_start]If an external system fails, cached or partial data is returned (RD3)[cite: 637]. |
| **Centralized SSO (OAuth2/OIDC)** | **(Security)** All user-facing channels authenticate via the institution’s SSO using OAuth2/OpenID Connect. [cite_start]Short-lived access tokens carry user identity and roles, strictly enforcing RS7 and RS8[cite: 629]. |

## Step 5: Instantiate Architectural Elements and Define Interfaces

This iteration refines key modules by assigning explicit responsibilities and defining the main interfaces between them.

| Element | Responsibility | Key Interfaces (Examples) |
| :--- | :--- | :--- |
| **Conversation/ API Gateway** | Exposes REST/GraphQL endpoints; performs request validation, token verification, rate limiting, and caching of frequent responses. | `handleRequest(request)`: Receives/normalizes messages.<br>`getCachedResponse(userId, queryKey)`: Retrieves cached data. |
| **Conversation Orchestrator** | [cite_start]Interprets user intents using AI models; coordinates with integration services; manages context and personalization (RS5)[cite: 628]. | `processMessage(userContext, message)`: Returns response object.<br>`enqueueTask(taskDescriptor)`: Offloads non-critical tasks. |
| **Integration Services** | [cite_start]Encapsulates access to external systems (LMS, Registration); handles protocol details, retries, and error mapping (RD2, RD3)[cite: 637]. | `getStudentSchedule(userId)`: Returns classes/exams.<br>`publishAnnouncement(courseId, content)`: Posts data to external systems. |
| **Identity & Access Mgmt (IAM)** | [cite_start]Validates SSO tokens; enforces role-based access control (RBAC) policies for Students, Lecturers, and Admins (RS7, RL8)[cite: 629, 631]. | `validateToken(token)`: Returns principal/roles.<br>`checkAccess(principal, resource)`: Returns allow/deny based on policy. |
| **Data Stores** | [cite_start]Persists interaction history for personalization (R2) [cite: 626][cite_start]; implements replication and backup for disaster recovery (RA6, RM6)[cite: 633, 635]. | `saveInteraction(userId, record)`: Persists exchanges.<br>`getRecentInteractions(userId)`: Supports context continuity. |

## Step 6: Sketch Views (Deployment Diagram)

The deployment view is refined to show stateless services as replicated containers behind a load balancer, with a separate message queue node and replicated data stores.



*(Note: Insert Deployment Diagram here)*

## Step 7: Analyze Design and Review Iteration

The following Kanban board summarizes the status of the drivers following this iteration.

| Not Addressed | Partially Addressed | Completely Addressed | Design Decisions Made |
| :--- | :--- | :--- | :--- |
| | | **Performance (RS10, R7)** | [cite_start]Achieved via Caching, Asynchronous Processing, and Elastic Scaling[cite: 626, 629]. |
| | | **Availability (RS11, RA6)** | [cite_start]Achieved via Active Redundancy, Load Balancing, and Database Replication[cite: 629, 633]. |
| | | **Security (RS7, RS8)** | [cite_start]Achieved via Centralized SSO, Token-based access, and RBAC[cite: 629]. |
| | **Operations (RM1-RM4)** | | [cite_start]Concepts for monitoring/logging are defined, but specific platform tools (e.g., Prometheus) are not yet selected[cite: 635]. |
| | **Data Sync (RD1-RD3)** | | [cite_start]Integration services are defined, but specific API contracts and sync schedules need detailed specification[cite: 637]. |
| **Usability (RS12)** | | | [cite_start]Detailed UI design is deferred to future iterations[cite: 629]. |
| **Multi-language (RS4)** | | | [cite_start]Translation services are not yet explicitly modeled[cite: 628]. |
