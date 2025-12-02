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

## 3. Design concepts for quality attributes

### 3.1 Performance

To meet the 2‑second average response time requirement under normal load, AIDAP adopts a layered set of performance tactics.  
Key design concepts are:  
- Caching: Frequently accessed data (course schedules, deadlines, announcements, user preferences) are cached at the Conversation/API Gateway and Conversation Orchestrator layers, with configurable TTL and cache invalidation based on updates from source systems.  
- Asynchronous processing: Non‑critical tasks such as detailed logging, analytics aggregation, and some notification fan‑out operations are handled via a message queue and background workers, so that user‑facing requests are not blocked by slow downstream processing.  
- Elastic scaling: Stateless services (gateway, orchestrator, integration services) can scale horizontally based on traffic, using container orchestration or cloud autoscaling policies aligned with R7 and RA7.  

These concepts are chosen because they reduce end‑to‑end latency while keeping the architecture compatible with cloud‑native deployment and monitoring requirements.

### 3.2 Availability

To achieve 99.5% monthly availability and support failover and recovery, the architecture strengthens redundancy and failure handling around critical services and data.  
Key design concepts are:  
- Redundancy and load balancing: Stateless services (API gateway, orchestrator, integration services) run in multiple active instances behind a load balancer, allowing traffic redistribution if an instance or node fails.  
- Resilient integration: Calls to external data source systems are wrapped in retry, timeout, and circuit‑breaker logic, with fallbacks such as returning cached or partial data when upstream systems are temporarily unavailable, as required by RD1–RD3.  
- Data protection and recovery: Persistent stores (interaction history, configuration, policy and role data, logs) are deployed with replication, automated backups, and point‑in‑time restore to align with RA6 and RM6.  

This combination of redundancy, resilience patterns, and backup strategies improves fault tolerance without significantly affecting performance.

### 3.3 Security

To protect user data and comply with institutional privacy policies, the architecture centralizes authentication and authorization while securing data in transit and at rest.  
Key design concepts are:  
- Centralized SSO and token‑based access: All user‑facing channels authenticate via the institution’s SSO using OAuth2/OpenID Connect, issuing short‑lived access tokens that carry user identity and roles required for RS7, RS8, RL8, and RM7.  
- Policy‑driven authorization and RBAC: An Authorization Service enforces role‑based access control and high‑level policies configured by administrators, ensuring that only authorized users can view or modify specific data.  
- Secure communication and storage: All inter‑service and client‑service traffic uses TLS, and sensitive data (user identifiers, interaction history, tokens, configuration secrets, logs with personal data) are encrypted at rest with strict key management.  

These tactics align with institutional security policies while supporting maintainers’ operational workflows and role‑based maintenance operations.

## 4. Instantiated elements, responsibilities, and interfaces

This iteration refines several key modules by assigning explicit responsibilities and defining the main interfaces between them, following the pattern used in the FCAPS case study.

- Conversation/API Gateway  
  - Responsibilities:  
    - Exposes REST/GraphQL endpoints and channel‑specific adapters (web, mobile, voice) for conversational interaction.  
    - Performs request validation, authentication token verification, basic rate limiting, and caching of frequently requested responses.  
  - Interfaces (examples):  
    - `handleRequest(request)`: Receives user messages and forwards normalized requests to the Conversation Orchestrator.  
    - `getCachedResponse(userId, queryKey)`: Retrieves cached responses when applicable.  

- Conversation Orchestrator  
  - Responsibilities:  
    - Interprets user intents using AI models, composes responses by coordinating with integration services and knowledge sources, and applies personalization based on interaction history.  
    - Enqueues non‑critical tasks (logging, analytics, some notifications) on a message queue for asynchronous processing.  
  - Interfaces (examples):  
    - `processMessage(userContext, message)`: Returns a response object including text, structured data, and any side effects (e.g., notifications).  
    - `enqueueTask(taskDescriptor)`: Sends a task to the message queue for background processing.  

- Integration Services (`LMSService`, `RegistrationService`, `CalendarService`, `EmailService`)  
  - Responsibilities:  
    - Encapsulate access to each external system using standard REST/GraphQL APIs, handling protocol details, retries, and error mapping.  
    - Expose simplified operations such as `getSchedule`, `getDeadlines`, `updateCourseContent`, and `postAnnouncement`.  
  - Interfaces (examples):  
    - `getStudentSchedule(userId)`: Returns upcoming classes and exams, used for RS1–RS3 and RS10.  
    - `publishAnnouncement(courseId, content, audience)`: Issues an announcement to students via integrated channels.  

- Identity and Access Management (IAM) / Authorization Service  
  - Responsibilities:  
    - Integrates with institutional SSO to authenticate users and issue tokens containing roles (Student, Lecturer, Administrator, Maintainer).  
    - Evaluates authorization policies for sensitive operations and enforces RS7–RS8, RL8, and administrative security requirements.  
  - Interfaces (examples):  
    - `validateToken(token)`: Returns authenticated principal and roles or an error.  
    - `checkAccess(principal, action, resource)`: Returns allow/deny with explanation based on current policies.  

- Data Stores (`InteractionHistoryStore`, `ConfigStore`, `MetricsStore`)  
  - Responsibilities:  
    - Persist historical interactions for personalization (R2, RS2–RS5), store configuration and policy data, and maintain metrics/logs for monitoring and compliance.  
    - Implement replication, backup, and recovery mechanisms consistent with availability and operations requirements.  
  - Interfaces (examples):  
    - `saveInteraction(userId, interactionRecord)`: Persists a conversational exchange for later personalization.  
    - `getRecentInteractions(userId, limit)`: Supports offline cache and context continuity.  

- Monitoring and Deployment Support Components  
  - Responsibilities:  
    - Collect latency, error, and availability metrics across services; expose dashboards for maintainers; integrate with continuous deployment pipelines.  
  - Interfaces (examples):  
    - `recordMetric(service, metricName, value)`: Logs performance and availability data.  
    - `triggerCanaryDeployment(versionId)`: Supports controlled rollouts with minimal downtime.  

## 5. Views and recorded design changes

The logical and deployment views produced in earlier iterations are updated conceptually by Iteration 3 decisions.  
Key refinements include:  
- Logical view: The Conversation/API Gateway, Conversation Orchestrator, Integration Services, IAM/Authorization Service, and Data Stores are explicitly annotated with caching, asynchronous messaging, and security responsibilities as described above.  
- Deployment view: Stateless services are shown as replicated containers or instances behind a load balancer, data stores are depicted with replication and backup components, and a message queue is introduced between the orchestrator and background workers.  

If diagrams are included in the repository, they should reflect these refinements and be referenced from this document (for example, “see Deployment Diagram v3 in `/diagrams/deployment-v3.png`”).

## 6. Analysis of iteration goal achievement

This iteration significantly improves support for performance, availability, and security while staying aligned with AIDAP’s cloud‑native and integration requirements.  
The following summarizes the coverage of the main drivers:

- Completely addressed  
  - RS10 (2‑second response time) through caching, asynchronous processing, and elastic scaling.  
  - RS11, RA6, RA7 (availability and scalability) through redundancy, load balancing, resilience patterns, and replicated data stores with backups.  
  - R8, RS7–RS8, RL8, and related security requirements (core security and access control) through SSO, token‑based access, RBAC, and encrypted communication and storage.  

- Partially addressed  
  - RM1–RM4, RM6 (operations and monitoring) are supported conceptually via monitoring, metrics, and deployment components, but further detailed operational procedures and specific platform choices remain to be defined.  
  - RD1–RD3 (data synchronization and interoperability) are addressed via integration services and resilience mechanisms, but detailed API contracts and sync schedules are left to future refinement.  

- Not addressed in this iteration  
  - Usability‑focused attributes (e.g., RS12 conversational UI quality) and multi‑language support (RS4) are still handled mainly by previous iterations and future design work.  

Overall, Iteration 3 achieves its primary goal of refining the architecture to better satisfy performance, availability, and security requirements for the AIDAP platform.

