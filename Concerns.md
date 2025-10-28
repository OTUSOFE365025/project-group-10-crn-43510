# Architectural Concerns for AIDAP

This document lists the key architectural concerns for the AIDAP platform, each mapped to explicit requirements from the project documentation. These concerns address design questions, challenges, and risks critical to meeting stakeholder needs and quality attributes.

| **Architectural Concern**                                        | **Type**               | **Stakeholders**      | **Related Req. IDs**  | **Notes / Rationale**                      |
|------------------------------------------------------------------|------------------------|-----------------------|-----------------------|---------------------------------------------|
| Sustaining <2s query response with live AI, data retrieval       | Performance            | S, L, A, M            | RS10, RA1             | Ensures natural, usable interactions        |
| Achieving 99.5% availability and automated fail-over             | Reliability            | S, L, A, M            | RS11, RA6             | Mission-critical service continuity         |
| Scaling for 5,000+ concurrent users                              | Scalability            | S, L, A, M            | RA7                   | Supports peak university load               |
| Enforcing SSO and user-specific data access                      | Security/Privacy       | S, L, A               | RS7, RS8, RA5         | Prevents unauthorized access                |
| Complying with privacy regulations & data retention policies     | Privacy/Governance     | S, A                  | RA2, RA5              | Meets FIPPA/GDPR, protects student data     |
| Preserving data integrity across institutional integrations      | Interoperability       | S, L, A, D            | RA4, RD2              | Accurate, synchronized information          |
| Supporting multi-channel (web, mobile, voice) consistent UX      | Usability/Portability  | S, L, A               | RS9, RS4, RA1         | Seamless experience on all platforms        |
| Facilitating zero-downtime updates and rollbacks                 | Maintainability        | M                     | RM1, RA6              | Reliable and safe deployments               |
| Instrumenting system health, latency, model accuracy             | Observability          | M, A                  | RS12, RM2             | Enables timely issue detection              |
| Admin-manageable policies for retention, logging, integrations   | Governance             | A, M                  | RA2, RA5, RA7         | Adaptable and auditable operations          |
| Resilient notification/announcement delivery                     | Fault Tolerance        | S, L, A               | RA6, RS2, RD3         | Keeps users informed despite failures       |
| Balancing AI-driven personalization with privacy controls        | Usability/Privacy      | S, A                  | RS5, RA2, RA5         | Relevant responses without overreach        |
| Handling offline cached data for limited connectivity            | Usability/Reliability  | S                     | RA9                   | Improves usability in weak networks         |
| Securing operational and admin functions via RBAC                | Security/Operations    | M, A                  | RA5, RS8              | Limits critical system modification access  |
| Traceable documentation and requirement mapping                  | Process/Documentation  | All                   | Project Guideline     | Ensures compliance and maintainability      |

**Legend:**  
- **Stakeholders:**  
  - S: Students  
  - L: Lecturers  
  - A: Administrators  
  - M: Maintainers  
  - D: Data source systems
