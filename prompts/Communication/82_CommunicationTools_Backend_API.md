# 82_CommunicationTools_Backend_API.md

**Core Feature:** Communication Tools (Reminders and Notifications) - Backend API Endpoints (NestJS)

**Purpose:** Define the RESTful API endpoints for managing Reminders, Notifications, and Communication Logs in the backend NestJS application.

**Instructions for AI Agent:**

*   **Generate NestJS controller code** with the API endpoints defined below.
*   Use `/reminders`, `/notifications`, and `/communication-logs` as base paths for controllers.
*   For each endpoint, generate:
    *   NestJS Controller decorator (`@Controller`)
    *   Route path decorators (`@Get`, `@Post`, `@Put`, `@Delete`)
    *   Method signature with parameters and types (TypeScript).
    *   Basic method body calling service methods (placeholders to services in `83_CommunicationTools_Backend_Services.md`).
    *   Appropriate HTTP status codes.
    *   Authentication (`@UseGuards(JwtAuthGuard)`). *Restrict reminder/notification management endpoints to appropriate roles (e.g., Receptionist, Admin).*
    *   Basic validation using NestJS `ValidationPipe` and DTOs.

---

## API Endpoints:

### Reminder Template Endpoints (`/reminders/templates`):

*   **POST /reminders/templates**: Create a new Reminder Template.
    *   Method: `POST`
    *   Request Body: `CreateReminderTemplateDto` (Define DTO with fields from `LookupReminderTemplates` table - template_name, template_type (ENUM: 'SMS', 'Email'), template_content, description - validation). *Restrict to Admin roles.*
    *   Response: `ReminderTemplateDto`, 201 Created.

*   **GET /reminders/templates**: Get a list of Reminder Templates.
    *   Method: `GET`
    *   Response: `ReminderTemplateDto[]`, 200 OK. *Restrict to Admin or authorized roles.*

*   **GET /reminders/templates/:templateId**: Get a specific Reminder Template by ID.
    *   Method: `GET`
    *   Path Parameter: `templateId` (UUID)
    *   Response: `ReminderTemplateDto`, 200 OK. *Restrict to Admin or authorized roles.*

*   **PUT /reminders/templates/:templateId**: Update an existing Reminder Template.
    *   Method: `PUT`
    *   Path Parameter: `templateId` (UUID)
    *   Request Body: `UpdateReminderTemplateDto` (Partial updates, validation). *Restrict to Admin roles.*
    *   Response: `ReminderTemplateDto`, 200 OK.

*   **DELETE /reminders/templates/:templateId**: Delete a Reminder Template.
    *   Method: `DELETE`
    *   Path Parameter: `templateId` (UUID)
    *   Response: 204 No Content. *Restrict to Admin roles.*


### Notification Type Endpoints (`/notifications/types`):

*   **GET /notifications/types**: Get a list of Notification Types.
    *   Method: `GET`
    *   Response: `NotificationTypeDto[]`, 200 OK. *Restrict to Admin or authorized roles.*
    *   *Notification types are likely pre-defined, so CRUD endpoints might not be needed initially. Focus on listing for now.*


### Appointment Reminder Endpoints (`/reminders/appointments`):

*   **POST /reminders/appointments/trigger/:appointmentId**: Manually trigger appointment reminders for a specific appointment.
    *   Method: `POST`
    *   Path Parameter: `appointmentId` (UUID)
    *   Response:  Success message (e.g., "{ message: 'Reminders triggered for appointment' }"), 200 OK. *Restrict to Receptionist or authorized roles.*
    *   Description:  Allows manual triggering of reminders (SMS, Email) for an appointment. *Initially, focus on manual trigger, automated scheduling will be in services.*

*   **GET /reminders/appointments/logs/:appointmentId**: Get communication logs for reminders sent for a specific appointment.
    *   Method: `GET`
    *   Path Parameter: `appointmentId` (UUID)
    *   Response: `CommunicationLogDto[]`, 200 OK. *Restrict to Receptionist or authorized roles.*


### Communication Log Endpoints (`/communication-logs`):

*   **GET /communication-logs**: Get a list of Communication Logs (with pagination, filtering, search, date range, communication type, status).
    *   Method: `GET`
    *   Query Parameters:
        *   Pagination: `page`, `pageSize`
        *   Filtering: `communicationType` (ENUM: 'SMS', 'Email', etc.), `status` (ENUM: 'Sent', 'Failed', etc.), `referenceType` (e.g., 'Appointment', 'Notification'), `referenceId` (UUID of related entity)
        *   Date Range: `startDate`, `endDate` (for sent/created dates)
        *   Search: *(Consider search fields - client name, patient name, message content?)*
        *   Sorting: (e.g., by `sent_at` timestamp)
    *   Response: `PaginatedResponse<CommunicationLogDto[]>`, 200 OK. *Restrict to Admin or authorized roles.*

*   **GET /communication-logs/:logId**: Get a specific Communication Log by ID.
    *   Method: `GET`
    *   Path Parameter: `logId` (UUID)
    *   Response: `CommunicationLogDto`, 200 OK. *Restrict to Admin or authorized roles.*

---