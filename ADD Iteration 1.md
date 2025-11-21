# ADD Iteration 1: Establishing System Foundation

## Step 1: Review Inputs
| **Category** | **Details** |
|--------------|-------------|
| **Design purpose** | This is a greenfield system from a mature domain and the purpose is to establish the overall architectural structure for the AI-Powered Digital Assistant Platform (AIDAP). The goal is to define high-level architectural decisions that support conversational interaction, AI-driven responses, and integration with institutional systems. |
| **Primary functional requirements** | From the use cases, the primary ones were identified as:<br><br>**UC-1:** Student AI Assistant — core functionality enabling students to query academic information.<br><br>**UC-2:** Notifications — supports real-time alerts for deadlines and announcements.<br><br>**UC-3:** Personalized Student Dashboard — provides contextual insights and student-specific data. |
| **Constraints** | The following constraints must be used as architectural drivers:<br><br>• Must integrate with university systems (SIS, LMS, Calendar).<br>• Must support **10,000+ concurrent users**.<br>• Must use a **cloud-based deployment model**. |
| **Architectural concerns** | The architect must keep in mind the following concerns:<br><br>• Scalability for a rapidly growing user base.<br>• Integration with external AI and NLP services.<br>• Data privacy, authentication, and compliance with institutional policies. |


### Quality Attribute Scenario:

| **Scenario ID** | **Importance** | **Difficulty of Implementation** |
|-----------------|----------------|----------------------------------|
| **QA-1**        | High           | High                             |
| **QA-2**        | High           | High                             |
| **QA-3**        | High           | Medium                           |
| **QA-4**        | Medium         | Medium                           |
| **QA-5**        | Medium         | Medium                           |
| **QA-6**        | High           | High                             |


## Step 2: Establish Iteration Goal
**Goal:** 
Design the high-level architecture to support core student-facing functionality (AI assistant, dashboard, notifications) while ensuring performance, availability, and security requirements are met.

**Focus Areas:**
- Overall system structure
- Major subsystems and their interactions
- Technology stack selection
- Deployment strategy

## Step 3: Choose Elements to Refine
**Elements Selected:**
- Entire AIDAP system (greenfield design)
- Focus on student-facing modules first
- Core AI integration layer

## Step 4: Choose Design Concepts

### Reference Architecture: Microservices Architecture with Three-Tier Layering

**Reference Architecture Selected:** Microservices-based Three-Tier Architecture

**Justification:**
The microservices architecture pattern is chosen as the primary reference architecture for AIDAP based on the following requirements analysis:
- Need for independent scaling of services (R7: cloud-native, scalable service)
- Integration with multiple external systems (R3: LMS, registration, calendars)
- Support for 5,000+ concurrent users (RA7)
- High availability requirements (RS11: 99.5% uptime)
- Need for independent deployment and updates (RM1: zero-downtime deployment)

Within each microservice, we apply the three-tier pattern (Presentation, Business Logic, Data) for internal organization.

**Reference:** This follows the microservices architecture pattern as described in "Building Microservices" by Sam Newman and the three-tier pattern from "Pattern-Oriented Software Architecture" (POSA).

### Architectural Pattern Details

**Tier 1: Presentation Layer**
- Handles user interaction for web, mobile, and voice interfaces
- Components: Student Interface, Lecturer Interface, Admin Interface, Maintainer Interface

**Tier 2: Business Logic Layer (Microservices)**
- Independent services with specific responsibilities
- Components: Authentication Service, AI Assistant Service, Dashboard Service, Notification Service, User Management Service

**Tier 3: Data Layer**
- Persistent storage and caching
- Components: PostgreSQL (structured data), MongoDB (conversation history), Redis (caching)

### Technology Decisions

**Frontend Technologies:**
- React.js for responsive web interface
- React Native for mobile applications
- Voice SDK integration for voice assistants

**Backend Technologies:**
- Node.js with Express framework for API services
- Microservices communicate via RESTful APIs
- Message queue (RabbitMQ) for asynchronous communication

**Data Storage:**
- PostgreSQL for relational data (users, courses, schedules, grades)
- MongoDB for unstructured data (conversation history, logs, analytics)
- Redis for session management and caching

**AI Integration:**
- OpenAI API for natural language processing (R5: AI models for NL queries)
- Custom intent classification for AIDAP-specific queries

**Authentication:**
- OAuth 2.0 with university SSO integration (RS7)
- JWT tokens for stateless authentication

**Cloud Infrastructure:**
- AWS for cloud hosting (R7: cloud-native deployment)
- Kubernetes for container orchestration
- Docker for containerization

**Justification for Technology Stack:**
- **React/React Native:** Unified codebase for web and mobile (RS9: mobile, web, voice)
- **Node.js:** JavaScript full-stack reduces context switching, excellent for I/O-heavy operations
- **Microservices:** Independent scaling and deployment aligns with university's need for high availability
- **PostgreSQL:** ACID compliance needed for academic data integrity (RD4)
- **MongoDB:** Flexible schema for conversation history (R2: store historical interactions)
- **Redis:** In-memory caching achieves 2-second response requirement (RS10)
- **OpenAI API:** Proven NLP capabilities without building AI from scratch (R5)
- **AWS + Kubernetes:** Industry-standard cloud-native deployment for scalability (R7, RA7)

## Step 5: Instantiate Architectural Elements

### AIDAP-Specific Components Created

#### Tier 1: Presentation Layer (Client Applications)
1. **AIDAP Student Web Application**
   - Web-based conversational interface for students
   - Personalized dashboard with academic information
   - Mobile-responsive design

2. **AIDAP Lecturer Web Application**
   - Course management interface
   - Class analytics and reporting
   - Announcement publishing

3. **AIDAP Admin Portal**
   - System administration interface
   - Integration management
   - Campus-wide announcement broadcasting

4. **AIDAP Maintainer Dashboard**
   - System health monitoring
   - Performance metrics
   - Deployment management

#### Tier 2: Business Logic Layer (AIDAP Microservices)

1. **University Authentication Service**
   - SSO integration with university identity provider
   - JWT token generation and validation
   - Role-based access control (Student, Lecturer, Admin, Maintainer)
   - Session management

2. **AIDAP Conversation Service** (AI Assistant)
   - Natural language query processing
   - Intent classification (schedule, grades, deadlines, course info)
   - OpenAI API integration
   - Conversation context management
   - Response generation with university-specific knowledge

3. **Academic Dashboard Service**
   - Student dashboard aggregation (grades, schedule, deadlines)
   - Lecturer dashboard (course analytics, pending work)
   - Admin dashboard (system usage, integration status)
   - Maintainer dashboard (health metrics, logs)
   - Personalization engine

4. **University Notification Service**
   - Multi-channel delivery (email, SMS, push, in-app)
   - Deadline reminders
   - Grade posting notifications
   - Schedule change alerts
   - Campus-wide announcements
   - Notification preference management

5. **Academic Data Integration Service**
   - LMS integration (courses, assignments, materials)
   - Registration system integration (schedules, enrollment)
   - Calendar integration (events, deadlines)
   - Email system integration
   - Synchronization management

6. **University User Management Service**
   - Student profile management
   - Lecturer profile management
   - Admin and maintainer account management
   - User preference storage
   - Role assignment

#### Tier 3: Data Layer (AIDAP Data Stores)

1. **University User Database (PostgreSQL)**
   - Student profiles and credentials
   - Lecturer and admin accounts
   - User preferences and settings
   - Role assignments

2. **Academic Data Database (PostgreSQL)**
   - Course catalog
   - Class schedules
   - Grade records
   - Enrollment data
   - Academic calendar

3. **AIDAP Conversation Store (MongoDB)**
   - Conversation history per student
   - Query and response logs
   - Intent classification results
   - Personalization data
   - Analytics data

4. **Session Cache (Redis)**
   - User session data
   - JWT tokens
   - Frequently accessed academic data
   - Query result caching
   - Real-time notification queue

#### Integration Layer (External System Connectors)

1. **LMS Connector**
   - Course material retrieval
   - Assignment and grade sync
   - Announcement publishing
   - Student progress tracking

2. **University Registration System Connector**
   - Class schedule retrieval
   - Enrollment status queries
   - Course availability checks

3. **University Calendar Connector**
   - Academic calendar sync
   - Deadline management
   - Event notifications

4. **Email Gateway Connector**
   - Email notification sending
   - Email integration for announcements

5. **OpenAI API Connector**
   - Natural language understanding
   - Response generation
   - Model version management
   - API key rotation

### Component Mapping to Requirements

| Component | Requirements Addressed |
|-----------|----------------------|
| AIDAP Conversation Service | RS1, RS5, R1, R5, R6 |
| Academic Dashboard Service | RS3, RL3, RA4 |
| University Notification Service | RS2, RL2, RL7, RA3 |
| University Authentication Service | RS7, RS8, RL8 |
| Academic Data Integration Service | R3, RD1, RD2, RD3 |
| University User Management Service | RS6, RL5, RM7 |
| Session Cache (Redis) | RS10, RS14 |
| Conversation Store (MongoDB) | R2, RS5 |


## Step 6: Sketch Views and Record Design Decisions

**Component Diagram:**
<img width="1599" height="697" alt="image" src="https://github.com/user-attachments/assets/f6cfcad6-00cc-48d2-86f8-f110a747232e" />



**Key Design Decisions:**
| Decision | Rationale | Trade-offs |
|----------|-----------|------------|
| Three-tier architecture | Clear separation, scalability | More complex than monolith |
| React frontend | Modern, component-based UI | Learning curve for team |
| Node.js backend | JavaScript full-stack, good for I/O | Not ideal for CPU-intensive tasks |
| Microservices approach | Independent scaling, maintainability | Increased deployment complexity |
| Cloud hosting (AWS) | Scalability, reliability | Ongoing costs, vendor lock-in |

## Step 7: Analyze Current Design and Review
**Requirements Addressed:**
- ✓ UC-1: Student AI Assistant (via AI Assistant Service)
- ✓ UC-3: Personalized Student Dashboard (via Dashboard Service)
- ✓ UC-2: Notifications (via Notification Service)

**Quality Attributes:**
- Performance: API Gateway and Redis caching support <2s response time
- Availability: AWS infrastructure with load balancing supports 99.5% uptime
- Security: OAuth 2.0 and JWT provide secure authentication

**Risks Identified:**
1. **Risk:** OpenAI API latency could affect response time
   - **Mitigation:** Implement caching for common queries
   
2. **Risk:** Database bottleneck under high load
   - **Mitigation:** Use read replicas and caching layer

3. **Risk:** Integration complexity with legacy university systems
   - **Mitigation:** Create abstraction layer for external integrations

**Next Iteration Focus:**
- Refine individual service designs
- Address lecturer and admin functionality
- Detailed sequence diagrams for key use cases
