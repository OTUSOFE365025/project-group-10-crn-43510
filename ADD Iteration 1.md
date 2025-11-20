# ADD Iteration 1: Establishing System Foundation

## Step 1: Review Inputs
**Design Purpose:** 
- Establish the overall system structure for AIDAP (AI Digital Assistant Platform)
- Focus on high-level architectural decisions

**Primary Functional Requirements:**
- UC-1: Student AI Assistant
- UC-3: Personalized Student Dashboard
- UC-2: Notifications

**Quality Attribute Scenarios:**
- Performance: System must respond to student queries within 2 seconds
- Availability: System must maintain 99.5% uptime during academic terms
- Security: User authentication and data protection must meet university standards

**Constraints:**
- Must integrate with existing university systems (student information system, learning management system)
- Must support 10,000+ concurrent users
- Cloud-based deployment required

**Architectural Concerns:**
- Scalability for growing user base
- Integration with external AI services
- Data privacy and compliance

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
**Architectural Pattern:** Three-tier architecture (Presentation, Business Logic, Data)

**Technology Decisions:**
- Frontend: React.js for responsive web interface
- Backend: Node.js with Express framework for API services
- Database: PostgreSQL for relational data, MongoDB for unstructured data
- AI Integration: OpenAI API for natural language processing
- Authentication: OAuth 2.0 with JWT tokens
- Hosting: AWS cloud infrastructure

**Justification:**
- Three-tier provides clear separation of concerns
- React enables responsive, modern UI
- Node.js offers good performance for concurrent requests
- Dual database approach handles both structured and unstructured data
- OpenAI provides robust AI capabilities without building from scratch
- AWS offers scalability and reliability

## Step 5: Instantiate Architectural Elements
**Components Created:**
1. **Presentation Layer:**
   - Student Web Interface
   - Lecturer Web Interface
   - Admin Web Interface

2. **Business Logic Layer:**
   - Authentication Service
   - AI Assistant Service
   - Notification Service
   - Dashboard Service
   - User Management Service

3. **Data Layer:**
   - User Database (PostgreSQL)
   - Conversation History Store (MongoDB)
   - Session Cache (Redis)

4. **Integration Layer:**
   - OpenAI API Connector
   - University System Integration Service
   - Email/SMS Gateway

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
