Quality Attributes for AIDAP

1. Performance
Definition:
The system must respond to user queries and perform key operations within acceptable time limits.
Rationale:
Since AIDAP is an AI-driven conversational assistant, quick and accurate responses are essential to maintaining a natural user experience and trust.
Scenarios:
•	UC-1: Ask Academic Question — responses must be generated within 2 seconds under normal load.
•	UC-2 / UC-7: Notification generation and delivery must occur within 1 minute of event detection.
•	UC-10: Dashboard metrics for maintainers should refresh in real-time (latency < 1s).
Metrics:
•	Average query latency < 2s
•	99.5% availability per month
•	Concurrent users supported: ≥ 5,000

2. Scalability
Definition:
The ability of the system to handle increasing numbers of users, requests, and data without degradation in performance.
Rationale:
AIDAP will serve multiple user groups (students, lecturers, admins) and must scale horizontally as the university population grows or during high-traffic periods (e.g., exam season).
Scenarios:
•	UC-1 / UC-2: During registration week, 3,000+ students may query simultaneously.
•	UC-9: Broadcasting campus-wide announcements to all users concurrently.
Design Implications:
•	Use containerized microservices deployed on the cloud (e.g., Kubernetes).
•	Implement load balancing and autoscaling.

3. Availability & Reliability
Definition:
The system must remain operational and deliver services consistently, minimizing downtime or service interruptions.
Rationale:
As an institutional assistant integrated with mission-critical systems, high uptime is essential for user trust and academic continuity.
Scenarios:
•	UC-1 to UC-9: The assistant must remain accessible 24/7 with 99.5% uptime per month.
•	UC-10: Failover mechanisms must ensure zero downtime during updates.
Design Implications:
•	Cloud deployment with redundancy zones.
•	Continuous monitoring and automated fail-over (RA6).
•	Automated recovery from node failures within 5 minutes.

4. Security
Definition:
Protection of data, communications, and system components from unauthorized access or misuse.
Rationale:
AIDAP handles sensitive academic data, so compliance with institutional security and privacy regulations is mandatory.
Scenarios:
•	UC-1 / UC-3 / UC-6: Authentication via institutional Single Sign-On (SSO).
•	UC-8: Only administrators can modify policies or integrations.
•	UC-10: Role-based access for maintainers and audit logging for every update.
Design Implications:
•	Use HTTPS/TLS for all API communications.
•	Implement RBAC (Role-Based Access Control).
•	Apply data encryption (at rest and in transit).
•	Ensure compliance with privacy acts (e.g., FIPPA, GDPR).

5. Usability
Definition:
The ease with which users of different roles can learn, use, and navigate the system effectively.
Rationale:
As a conversational system for non-technical users, the assistant must provide an intuitive, natural experience across devices.
Scenarios:
•	UC-1: Conversational interactions in natural language.
•	UC-4: Support for multiple languages and simple preference adjustments.
•	UC-3 / UC-6 / UC-8: Dashboards with clear visualizations and consistent design.
Metrics:
•	New users can perform a basic query within 1 minute.
•	Task success rate ≥ 90%.
•	Error message clarity (simple and instructive).

6. Maintainability
Definition:
Ease of updating, extending, and debugging the system.
Rationale:
The platform will evolve as new AI services and institutional systems are integrated.
Scenarios:
•	UC-10: Maintainers can deploy updates via continuous integration pipelines with zero downtime (RM1).
•	Adding new AI models or APIs requires minimal code changes.
Design Implications:
•	Modular microservices architecture.
•	Centralized configuration management.
•	Clear documentation and interface contracts (API specs).

7. Modifiability & Extensibility
Definition:
Ability to make controlled changes to system components or add new features without major redesign.
Rationale:
The assistant should easily adapt to new institutional policies or external systems.
Scenarios:
•	UC-10 / RD5: Integration of new AI models or external APIs without service disruption.
•	UC-8: Updating institutional rules or policies via admin dashboard.
Design Implications:
•	Plugin-based architecture for data sources.
•	Use of REST/GraphQL API standards.
•	Configuration-driven AI model selection.
8. Interoperability
Definition:
The capability to interact seamlessly with diverse external university systems.
Rationale:
AIDAP depends on connections to LMS, calendar, registration, and email systems.
Scenarios:
•	UC-8 / UC-10: Integration with LMS and calendar APIs (RD2).
•	UC-5: Exporting events to Google/Apple calendars using standard formats.
Design Implications:
•	Adhere to REST or GraphQL API specifications.
•	Implement retry logic and error handling for failed data syncs.
•	Ensure data consistency (RD4).

9. Privacy
Definition:
Protection of users’ personal and academic data from unauthorized disclosure.
Rationale:
Since students’ academic records and preferences are processed, privacy must be guaranteed.
Scenarios:
•	UC-3 / UC-4: Only authenticated users can access or modify personal data.
•	UC-8: Administrators manage data retention and logging policies (RA2).
Design Implications:
•	Compliance with institutional privacy acts.
•	Tokenized data for AI model training.
•	Minimal personal data storage.

10. Portability
Definition:
The ability of the system to operate across multiple platforms and devices.
Rationale:
Users should access AIDAP on web, mobile, and voice-assistant interfaces.
Scenarios:
•	UC-1 / UC-9: The same query interface should work via browser, smartphone app, or voice device.
Design Implications:
•	Develop front-end using responsive web design or cross-platform frameworks (e.g., Flutter/React Native).
•	Standardize APIs for front-end consumption.

11. Testability
Definition:
Ease with which system components can be tested to verify functionality and performance.
Rationale:
To ensure reliability of AI features and integrations before production rollout.
Scenarios:
•	UC-1 / UC-5: Test NLP interpretation accuracy and calendar sync.
•	UC-10: Automated health checks for latency, uptime, and response consistency.
Design Implications:
•	Unit and integration testing frameworks.
•	Mock APIs for external systems.
•	Continuous testing pipeline.

12. Fault Tolerance & Recoverability
Definition:
The ability of the system to continue operating correctly even in the presence of hardware, software, or network failures.
Rationale:
Institutional systems must not go down due to a single failure.
Scenarios:
•	UC-7 / UC-9: Continue sending notifications during temporary data source outages.
•	UC-10: Recovery from failed model deployment automatically.
Design Implications:
•	Retry mechanisms and circuit breakers.
•	Regular data backups and restore automation.
•	Graceful degradation and alerting.

In Summary
| **Quality Attribute**      | **Key Focus**                  | **Relevant Use Cases**  |
| -------------------------- | ------------------------------ | ----------------------- |
| Performance                | Response speed, low latency    | UC-1, UC-2, UC-10       |
| Scalability                | Handling large user loads      | UC-1, UC-9              |
| Availability & Reliability | Uptime, fault recovery         | UC-1–UC-10              |
| Security                   | Access control, encryption     | UC-1, UC-6, UC-8, UC-10 |
| Usability                  | Ease of interaction            | UC-1, UC-3, UC-4        |
| Maintainability            | Continuous deployment          | UC-10                   |
| Extensibility              | Adding new AI services         | UC-10, UC-8             |
| Interoperability           | Integration with LMS, calendar | UC-5, UC-8              |
| Privacy                    | Compliance & confidentiality   | UC-3, UC-8              |
| Portability                | Web, mobile, voice             | UC-1, UC-9              |
| Testability                | Ease of verifying behavior     | UC-1, UC-10             |
| Fault Tolerance            | Resilience to failures         | UC-7, UC-9, UC-10       |


