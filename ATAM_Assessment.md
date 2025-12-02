##ATAM Utility Tree

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
