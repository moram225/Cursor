# 92_AuditLogging_Backend_API.md

**Core Feature:** Audit Logging - Backend API Endpoints (NestJS)

**Purpose:** Define the RESTful API endpoints for retrieving and querying Audit Logs in the backend NestJS application.  *No API endpoints for creating, updating, or deleting audit logs as these should be system-generated.*

**Instructions for AI Agent:**

*   **Generate NestJS controller code** with the API endpoints defined below.
*   Use `/audit-logs` as the base path for the controller.
*   For each endpoint, generate:
    *   NestJS Controller decorator (`@Controller`) - with `/audit-logs` base path.
    *   Route path decorators (`@Get`) - Audit logs are primarily read-only data retrieval.
    *   Method signature with parameters and types (TypeScript).
    *   Basic method body calling service methods (placeholders to services in `93_AuditLogging_Backend_Services.md`).
    *   Appropriate HTTP status codes (200 OK for successful log retrieval).
    *   Authentication (`@UseGuards(JwtAuthGuard)`).  *Restrict access to Audit Log retrieval to Admin roles - `@Roles('Admin', 'Auditor', 'SuperAdmin')`.*
    *   Define DTOs for audit log responses to structure the data returned by each endpoint (`AuditLogDto`, `PaginatedResponse<AuditLogDto[]>`).

---

## API Endpoints (`/audit-logs`):

*   **GET /audit-logs**: Get a list of Audit Logs (with pagination, filtering, search, date range, user, action type, entity type).
    *   Method: `GET`
    *   Query Parameters:
        *   Pagination: `page`, `pageSize`
        *   Filtering: `userId`, `actionType` (ENUM of audit action types), `entityType` (ENUM of audited entity types), `entityId` (UUID of specific entity record), `eventStatus` (ENUM: 'Success', 'Failure', etc.)
        *   Date Range: `startDate`, `endDate` (for event timestamps)
        *   Search: *(Consider search fields - user name, action details, entity details?)*
        *   Sorting: (e.g., by `timestamp`, `actionType`, `entityType`)
    *   Response: `PaginatedResponse<AuditLogDto[]>`, 200 OK.
    *   Description: Retrieves a list of audit logs, supporting pagination and filtering.

*   **GET /audit-logs/:logId**: Get a specific Audit Log entry by ID.
    *   Method: `GET`
    *   Path Parameter: `logId` (UUID)
    *   Response: `AuditLogDto`, 200 OK.
    *   Description: Retrieves detailed information for a specific audit log entry based on its ID.

*   **(Potentially add endpoint for exporting audit logs in CSV/JSON format later - e.g., `GET /audit-logs/export?format=csv&...queryparams...`)**

---