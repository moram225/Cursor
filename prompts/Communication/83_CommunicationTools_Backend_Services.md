# 83_CommunicationTools_Backend_Services.md

**Core Feature:** Communication Tools (Reminders and Notifications) - Backend Services (NestJS)

**Purpose:** Define the NestJS services for managing Reminder Templates, Notifications, Communication Logs, and implementing reminder sending logic, interacting with Prisma ORM and simplified communication mechanisms.

**Instructions for AI Agent:**

*   **Generate NestJS service classes:** `ReminderTemplateService`, `NotificationTypeService`, `CommunicationLogService`, `CommunicationService` (or `ReminderService` which handles both template management and sending logic).
*   For each service method (corresponding to API endpoints in `82_CommunicationTools_Backend_API.md`):
    *   Method signature with parameters and return types (TypeScript).
    *   Prisma Client interactions for CRUD operations on `LookupReminderTemplates`, `LookupNotificationTypes`, `FactCommunicationLogs` tables.
    *   Implement logic in `CommunicationService` (or `ReminderService`) to:
        *   Retrieve reminder templates.
        *   Generate personalized reminder messages by populating templates with appointment and client data.
        *   *Implement simplified SMS sending logic (e.g., just log to console for now instead of actual SMS gateway integration).*
        *   *Implement simplified Email sending logic (e.g., using NestJS MailerModule but potentially configured to just log emails to console/file instead of actual sending initially).*
        *   Record communication logs in `FactCommunicationLogs` table, including status, timestamps, and any error details.
        *   Implement scheduling of appointment reminders (initially triggered manually via API, later automate using NestJS Tasks/Scheduling or external job scheduler).
    *   Error handling (Prisma exceptions, other service exceptions, HTTP exceptions).
    *   Logging.
    *   Mapping Prisma entities to DTOs.

---

## Service Definitions:

### ReminderTemplateService:

*   **createReminderTemplate(createReminderTemplateDto: CreateReminderTemplateDto): Promise<ReminderTemplateDto>**: Prisma `create` for `LookupReminderTemplates`.
*   **getReminderTemplates(): Promise<ReminderTemplateDto[]>**: Prisma `findMany` for `LookupReminderTemplates`.
*   **getReminderTemplateById(templateId: UUID): Promise<ReminderTemplateDto>**: Prisma `findUnique` for `LookupReminderTemplates` by `templateId`. `NotFoundException` if not found.
*   **updateReminderTemplate(templateId: UUID, updateReminderTemplateDto: UpdateReminderTemplateDto): Promise<ReminderTemplateDto>**: Prisma `update` for `LookupReminderTemplates`. `NotFoundException` if not found.
*   **deleteReminderTemplate(templateId: UUID): Promise<void>**: Prisma `delete` for `LookupReminderTemplates`. `NotFoundException` if not found.


### NotificationTypeService:

*   **getNotificationTypes(): Promise<NotificationTypeDto[]>**: Prisma `findMany` for `LookupNotificationTypes`. *(Initially, only listing)*


### CommunicationLogService:

*   **getCommunicationLogs(queryParams: GetCommunicationLogsQueryParams): Promise<PaginatedResponse<CommunicationLogDto[]>>**: Prisma `findMany`, `count` with pagination, filtering (on `communicationType`, `status`, `referenceType`, `referenceId`, date range, optional search), sorting.
*   **getCommunicationLogById(logId: UUID): Promise<CommunicationLogDto>**: Prisma `findUnique` for `FactCommunicationLogs` by `logId`. `NotFoundException` if not found.
*   **(Potentially add methods for creating communication logs directly from other services, e.g., `logCommunication(communicationLogData: CreateCommunicationLogDto): Promise<CommunicationLogDto>` - if needed for more direct logging from other services instead of just within `CommunicationService`)*


### CommunicationService (or ReminderService - for appointment reminders specifically):

*   **triggerAppointmentReminders(appointmentId: UUID): Promise<void>**:
    *   Implementation:
        *   Fetch `Appointment` details by `appointmentId` (including related `Patient`, `Client`, `DimLocations` etc.).
        *   Determine reminder channels (SMS, Email - based on client preference or clinic defaults - for now, assume both SMS and Email are sent).
        *   Retrieve appropriate reminder templates from `LookupReminderTemplates` (e.g., by `template_type` and potentially `reference_type` like 'AppointmentReminder').
        *   Personalize reminder messages by replacing placeholders in templates with appointment, client, patient details (e.g., clinic name, appointment time, patient name, client name).
        *   *Simplified SMS Sending:*  Log SMS message details (recipient, message content) to console.  Record communication log with status 'Sent' (or 'Pending' if asynchronous sending is considered later).
        *   *Simplified Email Sending:* Use NestJS MailerModule (configured for logging).  Send email using personalized template and data. Record communication log with status 'Sent' (or 'Pending').
        *   Handle potential errors during message generation or sending and log them in `FactCommunicationLogs` with status 'Failed'.


*   **(Potentially add more general notification sending methods later, e.g., `sendNotification(notificationType: NotificationTypeEnum, recipientUserId: UUID, data: any): Promise<void>` - for system-wide notifications triggered by various events.)*


---