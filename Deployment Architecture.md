# Deployment Architecture
### Click on the diagram for better visibility.

![Deployment Diagram](DeploymentDiagramPhase2.png)

| Element                             | Description                                                         | Domain Role                           |
|-------------------------------------|---------------------------------------------------------------------|---------------------------------------|
| Student Web Browser                 | React Frontend App for students                                     | Client Layer: Student access point    |
| Student Mobile Device               | React Native Mobile App (Student UI)                                | Client Layer: Mobile access for students |
| Lecturer Web Browser                | React Frontend App for lecturers                                    | Client Layer: Lecturer access point   |
| Admin Web Browser                   | React Frontend App for admins                                       | Client Layer: Admin access point      |
| Maintainer Dashboard Browser        | React Frontend App (Maintainer UI)                                  | Client Layer: System maintenance      |
| Edge/Gateway Layer (AWS)            | AWS Route 53 (DNS), AWS Application Load Balancer, AWS API Gateway  | Routing, load balancing, API ingress  |
| Application Layer (AWS EKS Cluster) | Authentication, Notification, AI Conversation, Academic Data Integration, Dashboard, User Management Services | Core business logic & application services |
| Caching Layer (AWS)                 | AWS ElastiCache Redis Cluster                                       | Session/state caching                 |
| Data Layer (AWS)                    | AWS RDS PostgreSQL Primary, Read Replicas, AWS DocumentDB Cluster   | Persistent data storage and retrieval |
| Monitoring/Operations Layer         | AWS CloudWatch                                                      | Performance & uptime monitoring       |
| Log Aggregation Service             | Log Collector/Aggregator                                            | Operational logging                   |
| External Integration Layer          | OpenAI API Server, University LMS Server, University Registration System Server, University Email Gateway Server | Integrations with 3rd-party/external systems |

