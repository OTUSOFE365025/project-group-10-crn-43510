## ATAM Utility Tree

The following Utility Tree identifies the driving architectural requirements for the AIDAP system. The scenarios are derived directly from the project requirements and prioritized based on their importance to the stakeholders and the difficulty of implementation.

| Quality Attribute | Attribute Refinement | ASR (Architectural Significant Requirement) Scenario | Priority (Importance, Difficulty) |
| :--- | :--- | :--- | :--- |
| **Availability** | Fault Tolerance | **(RS11)** The system shall remain available 99.5% of the time per month, ensuring consistent access for students and faculty. | (H, H) |
| | Recoverability | **(RA6)** The system shall provide high availability with automatic fail-over and backup recovery in the event of a system failure. | (H, M) |
| | Resilience | **(RD3)** The system shall handle failures in data source availability (e.g., LMS or Registration system downtime) gracefully using retry and recovery mechanisms. | (M, M) |
| **Security** | Data Confidentiality | **(RS8)** The system shall ensure that student-specific data (e.g., grades, schedules) are visible *only* to the authenticated user associated with that data. | (H, H) |
| | Authentication | **(RS7)** The system shall provide secure authentication through the institution's single sign-on (SSO) infrastructure. | (H, L) |
| | Access Control | **(RL8, RM7)** The system shall ensure role-based access control, preventing students from modifying course data and ensuring only maintainers perform updates. | (M, M) |
| **Performance** | Latency | **(RS10)** The system shall respond to user natural-language queries within 2 seconds on average under normal load. | (H, H) |
| | Throughput | **(RA7)** The system shall support scalability to handle performance requirements for up to 5,000 concurrent users (e.g., during course registration periods). | (H, H) |


![ATAM Utility Tree](ATAM%20Utility%20Tree.png)



## ATAM Risk Assessment Table

The following table maps the architectural decisions made in Iteration 3 to potential risks, sensitivities, and tradeoffs.
| Design Decision | Risk | Sensitivity | Tradeoff | Non-Risk |
| :--- | :--- | :--- | :--- | :--- |
| **Dependency on External AI Services (LLM)** | **(R1)** API latency from the AI provider might cause the system to violate the 2-second response time requirement (RS10) during peak load. | **(S1)** System latency is highly sensitive to the token count and complexity of the user's prompt. | **(T1)** Using a more capable model (e.g., GPT-4) increases response relevance (RS5) but increases latency and cost compared to smaller models. | **(NR1)** Using standard REST APIs for integration (RD2) is a low-risk, standard industry practice. |
| **Distributed Caching (Redis)** | **(R2)** Users may receive stale data (e.g., a grade changed moments ago) if the cache Time-To-Live (TTL) has not expired. | **(S2)** Overall system throughput (RA7) is sensitive to the Cache Hit Ratio; a low hit ratio spikes DB load. | **(T2)** We prioritize Performance (RS10) over strict real-time Data Consistency. We accept slight delays in data updates to ensure fast answers. | **(NR2)** Caching static content (course descriptions) is safe and carries no consistency risk. |
| **Centralized SSO Integration** | **(R3)** The system relies entirely on the University's Identity Provider. If the SSO system fails, AIDAP becomes inaccessible (Single Point of Failure). | **(S3)** Overall system availability (RS11) is directly sensitive to the SSO provider's uptime and configured timeout thresholds. | **(T3)** Enforcing strict SSO (RS7) ensures high security but increases coupling with external institutional infrastructure. | **(NR3)** Using OAuth2/OIDC is a standard, proven security protocol. |
| **Redundant, Stateless Architecture** | **(R4)** Misconfiguration of the replication strategy could lead to "split-brain" scenarios (data divergence) or excessive cloud costs due to over-provisioning. | **(S4)** Recovery time (RA6) is sensitive to the "Health Check" interval configured on the Load Balancer. | **(T4)** Achieving High Availability (RS11) increases infrastructure cost and operational complexity compared to a single-server deployment. | |
| **Strict RBAC & Encryption** | **(R5)** Incorrectly defined RBAC policies could accidentally block legitimate maintenance access (RM7) or degrade performance due to encryption overhead. | **(S5)** Administrative overhead is sensitive to the granularity of the roles defined (e.g., too many specific roles become hard to manage). | **(T5)** We trade processing speed (latency) for Data Confidentiality (RS8) by encrypting all data at rest and in transit. | **(NR4)** Using AES-256 for data at rest is a standard, compliant security measure (RA5). |

## Analysis of Risks, Sensitivities, and Tradeoffs

### Risks
* **R1 (Third-Party Latency):** The system's core value proposition relies on AI (R5), but we do not control the AI provider's uptime or speed. This is a significant risk to meeting **RS10** (Performance). *Mitigation:* We implemented Caching and Circuit Breakers in Iteration 3.
* **R3 (Auth Availability):** The requirement for SSO (RS7) creates a hard dependency. If the university's login server goes down, our availability metric (RS11) suffers even if our servers are running perfectly.
* **R4 (Split-Brain):** With database replication (RA6), network partitions can cause the Master and Slave to drift apart. If failover logic is too aggressive, we risk data corruption.

### Sensitivities
* **S1 (AI Token Count):** The time to process a request is not linear; it grows with the amount of history (context) we send to the AI. A student asking a question about a full semester's history will experience higher latency than a simple "Where is my class?" query.
* **S3 (SSO Dependence):** The system's perceived availability is tied 1:1 with the University's IdP. A 5-second timeout on the SSO validation could cascade into user login failures.

### Tradeoffs
* **T2 (Consistency vs. Performance):** To guarantee the **2-second response time (RS10)**, we chose to implement aggressive caching. This means we traded off **Data Consistency** (RD4). A student might see a notification for an event 1 minute after it was cancelled.
* **T5 (Performance vs. Confidentiality):** To ensure strict compliance with privacy laws (RS8), we encrypt all data. This introduces CPU overhead on every read/write, slightly impacting the latency budget (RS10) in favor of security.
