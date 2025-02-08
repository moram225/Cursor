# 93_AuditLogging_Backend_Services.md

**Core Feature:** Audit Logging - Backend Services (NestJS)

**Purpose:** Define the NestJS services for retrieving and storing Audit Logs, interacting with Prisma ORM.  Also, define the `AuditLogService` that will be used by other services to create audit log entries.

**Instructions for AI Agent:**

*   **Generate NestJS service classes:** `AuditLogQueryService` (for querying/retrieving audit logs), `AuditLogService` (for creating and logging audit events).  Potentially combine into a single `AuditLogService` if preferred.
*   For each service method in `AuditLogQueryService` (corresponding to API endpoints in `92_AuditLogging_Backend_API.md`):
    *   Method signature with parameters and return types (TypeScript).
    *   Prisma Client interactions for querying `FactAuditLogs` table with pagination, filtering, sorting, and searching.
    *   Error handling (Prisma exceptions, HTTP exceptions).
    *   Logging.
    *   Mapping Prisma entities to DTOs (`AuditLogDto`).

*   **Implement `AuditLogService` (for logging audit events):**
    *   Method `logEvent(auditLogData: CreateAuditLogDto): Promise<AuditLogDto>`:
        *   `CreateAuditLogDto` should include fields like `userId`, `actionType`, `entityType`, `entityId`, `eventStatus`, `details` (for storing 'before' and 'after' state or other relevant information).
        *   Implementation: Use Prisma Client to create a new record in `FactAuditLogs` table using data from `CreateAuditLogDto`.
        *   Return the created `AuditLogDto`.
    *   This `AuditLogService` should be designed to be easily injectable into other services (PatientService, AppointmentService, BillingService, etc.) so that they can log audit events.

*   **Integrate `AuditLogService` into existing services:**
    *   **Modify existing services (e.g., PatientService, AppointmentService, BillingService, ProductService, RoleService, UserRoleService) to use the `AuditLogService` to record audit events.**
    *   For each relevant service method (e.g., `createPatient`, `updateAppointment`, `deleteInvoice`, `createProduct`, `assignRoleToUser`), add code to:
        *   Inject `AuditLogService`.
        *   Construct a `CreateAuditLogDto` object with relevant information (current user ID, action type - e.g., 'Create Patient', 'Update Appointment', 'Delete Invoice', 'Create Product', 'Assign Role', entity type - 'Patient', 'Appointment', 'Invoice', 'Product', 'UserRole', entity ID, event status - 'Success', 'Failure' based on operation outcome, and potentially 'before' and 'after' data or other details).
        *   Call `auditLogService.logEvent(createAuditLogDto)` to record the audit event.
        *   *Start by logging CRUD operations (Create, Read, Update, Delete) for major entities. Login/Logout audit events and security-related events can be added later.*

---

## Service Definitions:

### AuditLogQueryService (or AuditLogService if combined):

*   **getAuditLogs(queryParams: GetAuditLogsQueryParams): Promise<PaginatedResponse<AuditLogDto[]>>**: Prisma `findMany`, `count` for `FactAuditLogs` with pagination, filtering (on `userId`, `actionType`, `entityType`, `entityId`, `eventStatus`, date range, optional search), sorting.
*   **getAuditLogById(logId: UUID): Promise<AuditLogDto>**: Prisma `findUnique` for `FactAuditLogs` by `logId`. `NotFoundException` if not found.


### AuditLogService (for event logging - if separated from Query Service):

*   **logEvent(auditLogData: CreateAuditLogDto): Promise<AuditLogDto>**: Prisma `create` for `FactAuditLogs`.

---