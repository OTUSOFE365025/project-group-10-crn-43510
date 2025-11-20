# Interface Specifications - Internal and External

## External Interfaces

### 1. OpenAI API Interface
**Type:** External REST API  
**Purpose:** Natural language processing for student queries

**Methods:**
| Method | Endpoint | Request Parameters | Response | Description |
|--------|----------|-------------------|----------|-------------|
| POST | /v1/chat/completions | model, messages[], temperature, max_tokens | JSON: choices[], usage | Send conversation to AI model and receive response |
| POST | /v1/embeddings | model, input[] | JSON: embeddings[], usage | Generate embeddings for semantic search |

**Authentication:** Bearer token (API key)  
**Protocol:** HTTPS  
**Data Format:** JSON

---

### 2. University SSO (Single Sign-On) Interface
**Type:** External OAuth 2.0  
**Purpose:** Student/lecturer/admin authentication

**Methods:**
| Method | Endpoint | Request Parameters | Response | Description |
|--------|----------|-------------------|----------|-------------|
| GET | /authorize | client_id, redirect_uri, response_type, scope | Authorization code | Initiate OAuth flow |
| POST | /token | grant_type, code, client_id, client_secret | JSON: access_token, refresh_token, expires_in | Exchange code for tokens |
| GET | /userinfo | access_token (header) | JSON: user profile data | Get authenticated user information |

**Authentication:** OAuth 2.0  
**Protocol:** HTTPS  
**Data Format:** JSON

---

### 3. LMS (Learning Management System) Interface
**Type:** External REST API  
**Purpose:** Access course materials, grades, assignments

**Methods:**
| Method | Endpoint | Request Parameters | Response | Description |
|--------|----------|-------------------|----------|-------------|
| GET | /api/courses | user_id | JSON: courses[] | Get list of enrolled courses |
| GET | /api/courses/{id}/assignments | course_id | JSON: assignments[] | Get assignments for a course |
| GET | /api/grades/{user_id} | user_id, course_id (optional) | JSON: grades[] | Get student grades |
| POST | /api/announcements | course_id, message, title | JSON: announcement_id | Post course announcement (lecturers) |

**Authentication:** API key or OAuth  
**Protocol:** HTTPS  
**Data Format:** JSON

---

### 4. University Registration System Interface
**Type:** External REST API  
**Purpose:** Access student schedules, course registration

**Methods:**
| Method | Endpoint | Request Parameters | Response | Description |
|--------|----------|-------------------|----------|-------------|
| GET | /api/schedule | student_id, term | JSON: schedule[] | Get student class schedule |
| GET | /api/courses/available | term, department (optional) | JSON: courses[] | Get available courses for registration |
| POST | /api/register | student_id, course_id | JSON: success, message | Register student for course |

**Authentication:** API key  
**Protocol:** HTTPS  
**Data Format:** JSON

---

## Internal Service Interfaces

### 1. Authentication Service Interface

**Service Name:** AuthenticationService  
**Responsibilities:** User authentication, authorization, token management

**Methods:**
| Method Name | Parameters | Return Type | Description |
|-------------|------------|-------------|-------------|
| login | username: string, password: string | AuthToken | Authenticate user credentials |
| logout | token: string | boolean | Invalidate user session |
| validateToken | token: string | TokenValidation | Verify JWT token validity |
| refreshToken | refresh_token: string | AuthToken | Generate new access token |
| getUserRole | user_id: string | Role | Get user role (Student, Lecturer, Admin, Maintainer) |
| checkPermission | user_id: string, resource: string, action: string | boolean | Check if user has permission for action |

**Data Models:**

interface AuthToken {
access_token: string;
refresh_token: string;
expires_in: number;
token_type: string;
}

interface TokenValidation {
valid: boolean;
user_id: string;
role: Role;
expires_at: Date;
}

enum Role {
STUDENT,
LECTURER,
ADMIN,
MAINTAINER
}


---

### 2. AI Assistant Service Interface

**Service Name:** AIAssistantService  
**Responsibilities:** Process natural language queries, manage conversation context

**Methods:**
| Method Name | Parameters | Return Type | Description |
|-------------|------------|-------------|-------------|
| processQuery | query: string, user_id: string, session_id: string | QueryResponse | Process user query and return AI response |
| getConversationHistory | user_id: string, session_id: string, limit: number | Conversation[] | Retrieve past conversation history |
| parseIntent | query: string | Intent | Classify query intent (schedule, grades, deadlines, etc.) |
| generateResponse | intent: Intent, context: Context, user_data: UserData | string | Generate contextual response |
| updateContext | session_id: string, new_context: Context | void | Update conversation context |

**Data Models:**

interface QueryResponse {
response: string;
confidence: number;
intent: Intent;
sources: DataSource[];
follow_up_suggestions: string[];
}

interface Conversation {
id: string;
user_id: string;
session_id: string;
query: string;
response: string;
timestamp: Date;
intent: Intent;
}

interface Intent {
type: string; // "schedule_query", "grade_query", "deadline_query", etc.
entities: Map<string, any>;
confidence: number;
}

interface Context {
previous_intent: Intent;
user_preferences: UserPreferences;
current_course: string;
conversation_history: string[];
}


---

### 3. Dashboard Service Interface

**Service Name:** DashboardService  
**Responsibilities:** Aggregate and personalize dashboard data for different user types

**Methods:**
| Method Name | Parameters | Return Type | Description |
|-------------|------------|-------------|-------------|
| getStudentDashboard | student_id: string | StudentDashboard | Get personalized student dashboard |
| getLecturerDashboard | lecturer_id: string | LecturerDashboard | Get personalized lecturer dashboard |
| getAdminDashboard | admin_id: string | AdminDashboard | Get admin dashboard with system metrics |
| getMaintainerDashboard | maintainer_id: string | MaintainerDashboard | Get system health and performance metrics |
| updateDashboardPreferences | user_id: string, preferences: DashboardPreferences | boolean | Update user dashboard customization |

**Data Models:**

interface StudentDashboard {
upcoming_classes: ClassEvent[];
upcoming_deadlines: Deadline[];
recent_grades: Grade[];
notifications: Notification[];
gpa: number;
quick_actions: Action[];
}

interface LecturerDashboard {
courses: Course[];
upcoming_classes: ClassEvent[];
pending_grading: Assignment[];
class_analytics: CourseAnalytics[];
announcements: Announcement[];
}

interface AdminDashboard {
system_usage: UsageMetrics;
active_users: number;
pending_requests: Request[];
system_alerts: Alert[];
integration_status: IntegrationStatus[];
}

interface MaintainerDashboard {
system_health: HealthMetrics;
performance_metrics: PerformanceMetrics;
error_logs: ErrorLog[];
active_deployments: Deployment[];
resource_utilization: ResourceMetrics;
}


---

### 4. Notification Service Interface

**Service Name:** NotificationService  
**Responsibilities:** Send notifications via multiple channels, manage subscriptions

**Methods:**
| Method Name | Parameters | Return Type | Description |
|-------------|------------|-------------|-------------|
| sendNotification | user_id: string, notification: Notification | boolean | Send notification to user |
| broadcastAnnouncement | announcement: Announcement, target_group: string | number | Broadcast to all users in group |
| subscribe | user_id: string, channel: NotificationChannel | boolean | Subscribe user to notification channel |
| unsubscribe | user_id: string, channel: NotificationChannel | boolean | Unsubscribe from channel |
| getNotificationHistory | user_id: string, limit: number | Notification[] | Get past notifications |
| updatePreferences | user_id: string, preferences: NotificationPreferences | boolean | Update notification preferences |

**Data Models:**

interface Notification {
id: string;
user_id: string;
type: NotificationType;
title: string;
message: string;
priority: Priority;
channels: NotificationChannel[];
timestamp: Date;
read: boolean;
}

enum NotificationType {
DEADLINE,
GRADE_POSTED,
SCHEDULE_CHANGE,
ANNOUNCEMENT,
SYSTEM_ALERT
}

enum NotificationChannel {
EMAIL,
SMS,
PUSH,
IN_APP
}

enum Priority {
LOW,
MEDIUM,
HIGH,
URGENT
}


---

### 5. User Management Service Interface

**Service Name:** UserManagementService  
**Responsibilities:** CRUD operations for user accounts, profile management

**Methods:**
| Method Name | Parameters | Return Type | Description |
|-------------|------------|-------------|-------------|
| createUser | user_data: UserCreateRequest | User | Create new user account |
| updateUser | user_id: string, updates: UserUpdateRequest | User | Update user profile |
| deleteUser | user_id: string | boolean | Soft delete user account |
| getUser | user_id: string | User | Get user details |
| searchUsers | query: string, role: Role | User[] | Search users by name/email |
| assignRole | user_id: string, role: Role | boolean | Assign role to user |
| updatePreferences | user_id: string, preferences: UserPreferences | boolean | Update user preferences |

**Data Models:**

interface User {
id: string;
username: string;
email: string;
first_name: string;
last_name: string;
role: Role;
created_at: Date;
last_login: Date;
preferences: UserPreferences;
}

interface UserPreferences {
language: string;
notification_settings: NotificationPreferences;
theme: string;
timezone: string;
}


---

## Internal Communication Protocols

**Between Services:**
- **Protocol:** HTTP/REST for synchronous requests
- **Message Queue:** RabbitMQ for asynchronous events
- **Data Format:** JSON
- **Authentication:** Service-to-service JWT tokens

**API Gateway Routes:**
- All client requests go through API Gateway
- API Gateway handles authentication, rate limiting, routing
- Services are not directly accessible from outside

---

## Summary

This document specifies all external and internal interfaces for the AIDAP system, ensuring clear contracts between components and external systems. All interfaces follow RESTful principles and use JSON for data exchange.


