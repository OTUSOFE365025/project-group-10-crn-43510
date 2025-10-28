# System Constraints for AIDAP

This file lists the core system constraints directly imposed by requirements for the AIDAP platform. Each constraint is mapped to explicit requirement IDs.

| **Constraint**                                    | **Type**         | **Related Requirement ID(s)** |
|---------------------------------------------------|------------------|-------------------------------|
| Cloud-native, scalable deployment                 | Infrastructure   | RA7                          |
| 2s avg query response time                        | Performance      | RS10, RA1                    |
| 99.5% monthly uptime                              | Reliability      | RS11, RA6                    |
| ≥5,000 concurrent users supported                 | Scalability      | RA7                          |
| Privacy and security compliance                   | Legal/Security   | RA2, RA5, RS8                |
| HTTPS/TLS required for communication              | Security         | RA5                          |
| SSO authentication enforced                       | Security         | RS7, RA5                     |
| Authorization: data visible only to authenticated | Authorization    | RS8, RA5                     |
| Standard REST/GraphQL APIs for integrations       | Interoperability | RA4, RD2                     |
| Real-time dashboard metrics                       | Observability    | RS12, RM2                    |
| Calendar event export support                     | Functionality    | RD5                          |
| Zero-downtime continuous deployment               | Maintainability  | RM1, RA6                     |
| Automated fail-over/recovery within 5min          | Reliability      | RA6                          |
| Modular microservices required                    | Design           | RA7, RM1                     |
| Role-based admin access control                   | Security         | RS8, RA5                     |
     
