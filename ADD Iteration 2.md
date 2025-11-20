# ADD Iteration 2: Refining Core Services

## Step 1: Review Inputs
**Design Purpose:** 
- Refine the core services identified in Iteration 1
- Address remaining use cases (lecturer, admin functionality)
- Ensure all quality attributes are properly addressed

**Primary Functional Requirements:**
- UC-4: Personalized Lecturer Dashboard
- UC-5: Administration Dashboard
- UC-6: Campus Wide Announcements
- UC-7: System Maintainer Dashboard

**Quality Attribute Scenarios:**
- Usability: Dashboards must be intuitive with <5 clicks to any function
- Modifiability: System must support adding new AI features without major refactoring
- Scalability: System must handle 50% growth in users without performance degradation

## Step 2: Establish Iteration Goal
**Goal:** 
Design detailed service architectures for lecturer, admin, and maintainer functionality while ensuring the system is modifiable, usable, and can scale effectively.

## Step 3: Choose Elements to Refine
**Elements Selected:**
1. Dashboard Service (expand for multiple user types)
2. Notification Service (add campus-wide announcements)
3. Admin/Maintenance modules
4. AI Assistant Service internal structure

## Step 4: Choose Design Concepts
**Design Patterns Applied:**

1. **Strategy Pattern** for AI Assistant Service
   - Allows switching between different AI providers
   - Supports custom AI models for specific use cases

2. **Observer Pattern** for Notification Service
   - Users subscribe to notification channels
   - Supports real-time updates via WebSockets

3. **Repository Pattern** for Data Access
   - Abstracts database operations
   - Makes it easy to switch databases if needed

4. **Facade Pattern** for External Integrations
   - Simplifies interaction with complex university systems
   - Isolates integration complexity

## Step 5: Instantiate Architectural Elements

**Refined Service Structure:**

### Dashboard Service Components:
- Student Dashboard Controller
- Lecturer Dashboard Controller
- Admin Dashboard Controller
- Maintainer Dashboard Controller
- Dashboard Data Aggregator
- Personalization Engine

### AI Assistant Service Components:
- Query Parser
- Context Manager
- AI Provider Interface (Strategy Pattern)
  - OpenAI Provider
  - Custom Model Provider (future)
- Response Formatter
- Conversation History Manager

### Notification Service Components:
- Notification Publisher
- Subscription Manager
- Delivery Router (email, SMS, push, in-app)
- Campus Announcement Module
- Notification Template Engine

### Admin Service Components:
- User Management Module
- System Configuration Module
- Analytics and Reporting Module
- Audit Log Manager

### Maintainer Service Components:
- System Health Monitor
- Log Aggregator
- Backup/Recovery Manager
- Performance Dashboard

## Step 6: Sketch Views and Record Design Decisions

**Detailed Service Architecture:**

- To be added

**Key Design Decisions:**

| Decision | Rationale | Trade-offs |
|----------|-----------|------------|
| Strategy pattern for AI providers | Flexibility to change/add AI services | Additional abstraction layer |
| Observer pattern for notifications | Real-time updates, scalable | Requires WebSocket infrastructure |
| Separate dashboards per user type | Customized UX, security isolation | Code duplication risk |
| Repository pattern for data | Database agnostic, testable | Performance overhead |
| Role-based access control (RBAC) | Fine-grained permissions | Complex permission management |

## Step 7: Analyze Current Design and Review

**All Requirements Addressed:**
- ✓ UC-1: Student AI Assistant
- ✓ UC-2: Notifications  
- ✓ UC-3: Personalized Student Dashboard
- ✓ UC-4: Personalized Lecturer Dashboard
- ✓ UC-5: Administration Dashboard
- ✓ UC-6: Campus Wide Announcements
- ✓ UC-7: System Maintainer Dashboard

**Quality Attributes Addressed:**

| Attribute | How Addressed | Verification |
|-----------|---------------|--------------|
| Performance | Redis caching, database indexing, API Gateway throttling | Load testing with 10k concurrent users |
| Availability | AWS auto-scaling, multi-AZ deployment, health monitoring | 99.5% uptime SLA monitoring |
| Security | OAuth 2.0, JWT, RBAC, encrypted data storage | Security audit, penetration testing |
| Usability | Role-specific dashboards, intuitive navigation | User testing, click-path analysis |
| Modifiability | Strategy pattern, loose coupling, microservices | Code review, dependency analysis |
| Scalability | Horizontal scaling, load balancers, stateless services | Stress testing, performance benchmarks |

**Risks Addressed from Iteration 1:**
1. ✓ OpenAI latency → Caching layer implemented
2. ✓ Database bottleneck → Read replicas and Redis cache
3. ✓ Integration complexity → Facade pattern isolates complexity

**Remaining Concerns:**
1. **Cost Management:** AWS services can be expensive at scale
   - Monitor usage, implement cost alerts
   
2. **AI Model Training:** Custom AI models may require significant effort
   - Start with OpenAI, evaluate ROI before custom development

3. **Data Privacy Compliance:** Must ensure GDPR/FERPA compliance
   - Implement data retention policies, user data export features

**Next Steps:**
- Create detailed sequence diagrams for each use case
- Design database schemas
- Develop deployment architecture
- Plan security implementation details
