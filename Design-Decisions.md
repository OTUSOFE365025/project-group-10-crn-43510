# Design Decisions - Iterations 1 & 2

## Design Decisions Table

| Iteration | Design Decision | Justification | Impact | Trade-offs |
|-----------|----------------|---------------|--------|------------|
| 1 | Three-tier architecture (Presentation, Business Logic, Data) | Clear separation of concerns, industry standard, supports scalability | Enables independent development of each layer, easier testing and maintenance | More complex than monolithic, requires more initial setup |
| 1 | React.js for frontend | Modern component-based UI framework, large ecosystem, good performance | Responsive user interface, reusable components, active community support | Learning curve for developers unfamiliar with React |
| 1 | Node.js with Express for backend | JavaScript full-stack development, excellent for I/O operations, non-blocking | Unified language across stack, good performance for concurrent requests | Not ideal for CPU-intensive operations |
| 1 | PostgreSQL for relational data | ACID compliance, robust for structured data, mature ecosystem | Reliable data integrity, complex queries, good performance | Requires schema definition upfront |
| 1 | MongoDB for unstructured data | Flexible schema, good for conversation history and logs | Easy to store varying document structures, horizontal scaling | Eventual consistency, less strict data integrity |
| 1 | Redis for caching | In-memory storage, extremely fast, supports various data structures | Sub-millisecond response times, reduces database load | Volatile storage, requires persistence configuration |
| 1 | OAuth 2.0 with JWT tokens | Industry standard for authentication, stateless, secure | No server-side session storage, scalable authentication | Token management complexity, token refresh needed |
| 1 | AWS cloud hosting | Scalable infrastructure, high availability, managed services | Auto-scaling, reliability, global reach | Vendor lock-in, ongoing costs, complexity |
| 1 | Microservices architecture | Independent services, isolated failures, technology flexibility | Can scale services independently, easier to update individual services | Increased complexity, network latency, distributed system challenges |
| 1 | API Gateway pattern | Single entry point, centralized security, request routing | Simplified client integration, centralized authentication, load balancing | Single point of failure if not properly redundant |
| 2 | Strategy pattern for AI providers | Flexibility to swap AI services without code changes | Easy to switch between OpenAI, custom models, or other providers | Additional abstraction layer adds complexity |
| 2 | Observer pattern for notifications | Real-time updates, scalable notification system | Users receive instant updates, supports multiple delivery channels | Requires WebSocket infrastructure, connection management |
| 2 | Repository pattern for data access | Abstract database operations, database-agnostic code | Easy to switch databases, better testability | Performance overhead from abstraction layer |
| 2 | Facade pattern for external integrations | Simplify complex external system interactions | Isolates integration complexity, easier to maintain | Additional layer between service and external systems |
| 2 | Role-based access control (RBAC) | Fine-grained permissions, security isolation | Different access levels for students, lecturers, admins, maintainers | Complex permission management, harder to debug access issues |
| 2 | Separate dashboard controllers per user type | Customized user experience, security isolation | Tailored functionality for each role, better UX | Potential code duplication, more controllers to maintain |
| 2 | WebSocket for real-time notifications | Bi-directional communication, instant updates | Real-time user experience, efficient for push notifications | Stateful connections, scaling considerations |
| 2 | Database read replicas | Distribute read load, improve performance | Better performance for read-heavy workloads, availability during maintenance | Eventual consistency, replication lag, increased cost |
| 2 | Circuit breaker for external services | Prevent cascading failures, graceful degradation | System remains partially functional when external services fail | Additional logic complexity, fallback handling needed |
| 2 | Containerization (Docker) | Consistent environments, easy deployment, portability | Same environment in dev/test/prod, rapid scaling | Learning curve, orchestration complexity |

## Decision Categories

### Architecture Patterns
- Three-tier architecture
- Microservices architecture
- API Gateway pattern

### Design Patterns
- Strategy pattern (AI providers)
- Observer pattern (notifications)
- Repository pattern (data access)
- Facade pattern (external integrations)
- Circuit breaker pattern (fault tolerance)

### Technology Stack
- Frontend: React.js
- Backend: Node.js + Express
- Databases: PostgreSQL, MongoDB, Redis
- Cloud: AWS
- Containerization: Docker

### Security & Access
- OAuth 2.0 + JWT
- Role-based access control (RBAC)
- Centralized authentication

### Performance & Scalability
- Caching (Redis)
- Database read replicas
- Load balancing
- Auto-scaling
- WebSocket for real-time updates

## Impact Summary

**Positive Impacts:**
- Scalable architecture supporting future growth
- Secure authentication and authorization
- High performance through caching and optimization
- Flexible design allowing technology changes
- Good separation of concerns for maintainability

**Challenges:**
- Increased complexity from microservices
- Higher initial infrastructure costs
- Learning curve for some technologies
- Need for robust monitoring and debugging tools
- Distributed system complexity

## Rationale for Key Decisions

### Why Microservices over Monolith?
- Independent scaling of services based on demand
- Team can work on different services in parallel
- Failure isolation (one service failing doesn't crash entire system)
- Technology flexibility for future services

### Why Multiple Databases?
- PostgreSQL for structured data requiring ACID compliance (users, courses, grades)
- MongoDB for flexible schema data (conversation history, logs)
- Redis for caching and session management (performance)
- Right tool for the right job approach

### Why Cloud Hosting (AWS)?
- University can scale during peak registration periods
- High availability with multi-AZ deployment
- Managed services reduce operational burden
- Global reach for potential future expansion
