# Logical Architecture

This section documents the logical architecture of the AIDAP platform, showing all major components, their responsibilities, and relationships. The diagram and table below strictly use names and descriptions as in the repo and project requirements.

---

## Logical Architecture Diagram

![Logical Architecture Diagram](logical architecture diagram.png)  
<sup>PlantUML source included below for transparency and reproducibility.</sup>

---

## Logical Architecture Table

| Element                                       | Description                                                            | Domain Role                             |
|-----------------------------------------------|------------------------------------------------------------------------|-----------------------------------------|
| AIDAP Student Web Application                 | Web-based conversational interface for students.                       | Student-facing portal                   |
| AIDAP Lecturer Web Application                | Web dashboard for lecturers to manage courses and analytics.           | Lecturer-facing portal                  |
| AIDAP Admin Portal                            | Admin interface for platform management and oversight.                 | Administrative control                  |
| AIDAP Maintainer Dashboard                    | Dashboard for maintainers to monitor and maintain system operations.   | Maintenance and support portal          |
| API Gateway                                   | Entry point for all requests, routes API calls to internal services.   | Request routing, security               |
| AIDAP Conversation Service                    | Processes AI queries; supports NLP and student/faculty interactions.   | AI-powered assistant engine             |
| Academic Dashboard Service                    | Aggregates and presents course analytics, grades, performance data.    | Visualization and analytics             |
| University Notification Service               | Sends announcements, reminders, and alerts to users.                   | Communication/messaging                 |
| Academic Data Integration Service             | Integrates data from LMS, registration, calendar systems, etc.         | Data aggregation and integration        |
| University User Management Service            | Handles account creation, profile updates, roles, and permissions.     | User/group administration               |
| University Authentication Service             | Manages logins, identity, and authentication tokens (JWT, SSO, etc).   | User authentication/security            |
| LMS Connector                                 | Middleware for connecting to Learning Management System APIs.           | Academic content integration            |
| University Registration System Connector      | Middleware for connecting to university registration database/API.      | Enrollment and records integration      |
| University Calendar Connector                 | Middleware for integrating institutional calendar events.               | Calendar syncing and event management   |
| Email Gateway Connector                       | Handles all email notifications to/from users and external systems.     | Email alerts, mailing                   |
| OpenAI API Connector                          | Integrates external AI/NLP processing for advanced conversational tasks | External AI service integration         |
| University User Database (PostgreSQL)         | Secure database storing core user information and profile data.         | Persistent user data storage            |
| Academic Data Database (PostgreSQL)           | Stores course schedules, grades, and academic analytics data.           | Persistent academic data storage        |
| AIDAP Conversation Store (MongoDB)            | Stores conversation and query history for students/faculty.             | Session/query persistence               |
| Session Cache (Redis)                         | Caches session information and frequently accessed records.             | Performance (speed, scalability)        |

