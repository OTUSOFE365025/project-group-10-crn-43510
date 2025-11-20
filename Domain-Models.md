# AIDAP Domain-Specific Models

This document defines the domain-specific data models for the AI-Powered Digital Assistant Platform (AIDAP).

## Core Domain Entities

### 1. Student
Represents a student user in the AIDAP system.

**Attributes:**
- student_id: string (primary key)
- university_id: string (unique)
- first_name: string
- last_name: string
- email: string
- program: string (e.g., "Computer Science", "Engineering")
- year: integer (1-4)
- gpa: decimal
- enrolled_courses: Course[] (list of courses)
- preferences: StudentPreferences
- created_at: timestamp
- last_active: timestamp

**Relationships:**
- Has many Conversations
- Has many Enrollments
- Has many Notifications
- Belongs to one Program

---

### 2. Lecturer
Represents a lecturer/faculty member.

**Attributes:**
- lecturer_id: string (primary key)
- university_id: string (unique)
- first_name: string
- last_name: string
- email: string
- department: string
- title: string (e.g., "Professor", "Assistant Professor")
- office_location: string
- office_hours: OfficeHours[]
- created_at: timestamp

**Relationships:**
- Teaches many Courses
- Creates many Announcements
- Has access to CourseAnalytics

---

### 3. Course
Represents an academic course.

**Attributes:**
- course_id: string (primary key)
- course_code: string (e.g., "SOFE3650")
- course_name: string
- department: string
- credits: integer
- semester: string (e.g., "Fall 2025")
- lecturer_id: string (foreign key)
- schedule: ClassSchedule[]
- max_enrollment: integer
- current_enrollment: integer
- syllabus_url: string

**Relationships:**
- Taught by one Lecturer
- Has many Enrollments
- Has many Assignments
- Has many Announcements

---

### 4. Conversation
Represents a conversation session between a user and AIDAP.

**Attributes:**
- conversation_id: string (primary key)
- user_id: string (foreign key)
- session_id: string
- messages: Message[] (array of messages)
- started_at: timestamp
- last_updated: timestamp
- context: ConversationContext
- status: string ("active", "closed")

**Relationships:**
- Belongs to one User (Student, Lecturer, Admin)
- Contains many Messages

---

### 5. Message
Represents a single query-response pair in a conversation.

**Attributes:**
- message_id: string (primary key)
- conversation_id: string (foreign key)
- query: string (user's question)
- response: string (AIDAP's answer)
- intent: Intent
- confidence: decimal (0.0 to 1.0)
- timestamp: timestamp
- data_sources: DataSource[] (which systems were queried)
- processing_time_ms: integer

**Relationships:**
- Belongs to one Conversation
- Has one Intent
- References multiple DataSources

---

### 6. Intent
Represents the classified intent of a student query.

**Attributes:**
- intent_id: string (primary key)
- intent_type: string (e.g., "schedule_query", "grade_query", "deadline_query")
- entities: Map<string, any> (extracted entities like course names, dates)
- confidence: decimal
- requires_authentication: boolean
- requires_external_data: boolean

**Intent Types:**
- schedule_query: "When is my next class?"
- grade_query: "What's my grade in SOFE3650?"
- deadline_query: "When is the assignment due?"
- course_info_query: "Tell me about SOFE3650"
- enrollment_query: "Am I enrolled in this course?"
- announcement_query: "Any new announcements?"
- general_query: "How do I drop a course?"

---

### 7. Assignment
Represents a course assignment or exam.

**Attributes:**
- assignment_id: string (primary key)
- course_id: string (foreign key)
- title: string
- description: string
- due_date: timestamp
- points_possible: integer
- assignment_type: string ("homework", "exam", "project", "quiz")
- submission_url: string
- created_at: timestamp

**Relationships:**
- Belongs to one Course
- Has many Submissions
- Generates Notifications (deadline reminders)

---

### 8. Grade
Represents a student's grade for an assignment or course.

**Attributes:**
- grade_id: string (primary key)
- student_id: string (foreign key)
- course_id: string (foreign key)
- assignment_id: string (foreign key, nullable)
- score: decimal
- points_possible: decimal
- letter_grade: string (nullable, for course grades)
- graded_at: timestamp
- comments: string

**Relationships:**
- Belongs to one Student
- Belongs to one Course
- May belong to one Assignment

---

### 9. Notification
Represents a notification sent to a user.

**Attributes:**
- notification_id: string (primary key)
- user_id: string (foreign key)
- type: NotificationType
- title: string
- message: string
- priority: Priority ("low", "medium", "high", "urgent")
- channels: NotificationChannel[] ("email", "sms", "push", "in-app")
- sent_at: timestamp
- read_at: timestamp (nullable)
- related_entity_id: string (e.g., assignment_id, course_id)
- related_entity_type: string

**Notification Types:**
- DEADLINE_REMINDER
- GRADE_POSTED
- SCHEDULE_CHANGE
- COURSE_ANNOUNCEMENT
- SYSTEM_ALERT
- CAMPUS_ANNOUNCEMENT

**Relationships:**
- Belongs to one User
- May reference an Assignment, Course, or Announcement

---

### 10. Announcement
Represents an announcement (course-level or campus-wide).

**Attributes:**
- announcement_id: string (primary key)
- title: string
- content: string
- author_id: string (lecturer_id or admin_id)
- scope: string ("course", "department", "campus")
- course_id: string (nullable, for course announcements)
- priority: Priority
- published_at: timestamp
- expires_at: timestamp (nullable)

**Relationships:**
- Created by one Lecturer or Admin
- May belong to one Course
- Generates many Notifications

---

### 11. Schedule
Represents a class session in a student's schedule.

**Attributes:**
- schedule_id: string (primary key)
- course_id: string (foreign key)
- day_of_week: string ("Monday", "Tuesday", etc.)
- start_time: time
- end_time: time
- location: string (building + room)
- recurrence: string ("weekly", "biweekly")
- start_date: date
- end_date: date

**Relationships:**
- Belongs to one Course
- Appears in many StudentSchedules

---

### 12. Enrollment
Represents a student's enrollment in a course.

**Attributes:**
- enrollment_id: string (primary key)
- student_id: string (foreign key)
- course_id: string (foreign key)
- enrolled_at: timestamp
- status: string ("active", "dropped", "completed")
- final_grade: string (nullable)

**Relationships:**
- Links one Student to one Course

---

## Supporting Domain Models

### StudentPreferences
User customization preferences.

**Attributes:**
- language: string ("en", "fr", etc.)
- notification_email: boolean
- notification_sms: boolean
- notification_push: boolean
- notification_in_app: boolean
- timezone: string
- theme: string ("light", "dark")
- dashboard_widgets: string[] (enabled widgets)

---

### ConversationContext
Maintains context for ongoing conversations.

**Attributes:**
- current_topic: string (e.g., "grades", "schedule")
- mentioned_courses: string[] (courses mentioned in conversation)
- mentioned_dates: date[] (dates mentioned)
- user_preferences: StudentPreferences
- previous_queries: string[] (last 5 queries)
- session_start_time: timestamp

---

### DataSource
Tracks which external systems were queried.

**Attributes:**
- source_name: string ("LMS", "Registration", "Calendar")
- query_timestamp: timestamp
- response_time_ms: integer
- success: boolean
- error_message: string (nullable)

---

### CourseAnalytics
Analytics data for lecturers about their courses.

**Attributes:**
- course_id: string
- total_students: integer
- average_grade: decimal
- assignment_completion_rate: decimal
- attendance_rate: decimal (if tracked)
- engagement_score: decimal
- generated_at: timestamp

---

## Domain Model Relationships Diagram

- To be added


## Key Domain Rules

1. **Student Enrollment:** A student must be enrolled in a course to access its materials and grades
2. **Conversation Context:** Each conversation maintains context to provide relevant responses
3. **Notification Preferences:** Users can customize which notification channels they receive
4. **Grade Privacy:** Students can only view their own grades
5. **Announcement Scope:** Course announcements only visible to enrolled students
6. **Intent Classification:** All queries must be classified to an intent before processing
7. **Data Source Integration:** Responses should cite which external systems provided the data

## Domain Events

Events that trigger actions in the AIDAP system:

- **GradePosted:** Triggers notification to student
- **AssignmentDue:** Triggers deadline reminder notification
- **ScheduleChanged:** Triggers schedule change notification
- **AnnouncementPublished:** Triggers announcement notification
- **EnrollmentCompleted:** Grants access to course materials
- **ConversationStarted:** Initializes conversation context

---

This domain model provides a comprehensive view of the AIDAP system's core entities and their relationships, ensuring the architecture properly represents the university academic domain.