# 90_AuditLogging_Overview.md

**Core Feature:** Audit Logging

**Purpose:** This feature implements comprehensive audit logging within the Veterinary PMS. It aims to record significant actions performed by users, system events, and data changes, providing a detailed audit trail for security, compliance, troubleshooting, and operational monitoring.  Audit logs will be crucial for tracking who did what, when, and where within the system. This feature should be integrated across all backend API endpoints and services to capture relevant audit events.

**User Stories:**

*   As a **Clinic Administrator**, I want to be able to **view audit logs** to track user activity and system events.
*   As a **Clinic Administrator**, I want to be able to **filter audit logs** by user, date range, action type, and affected data entity to easily find specific events.
*   As a **Clinic Administrator**, I want to be able to **generate reports on audit logs** for compliance and security audits.
*   As a **Security Officer/Auditor**, I need a **detailed and tamper-proof record of all important actions** within the system for security investigations and compliance audits.
*   As a **System Administrator**, I want to use audit logs to **troubleshoot system errors and identify the cause of unexpected behavior**.
*   As a **Developer**, I can use audit logs to **debug issues and understand the sequence of events leading to a problem**.

**Key Functionalities:**

*   **Event Logging:**
    *   Capture a wide range of audit events, including:
        *   User login and logout attempts (successful and failed).
        *   CRUD operations on key data entities (Clients, Patients, Appointments, Invoices, Products, etc.).
        *   Changes to system settings and configurations.
        *   Security-related events (permission changes, role modifications, access denials).
        *   Important workflow events (e.g., invoice sent, appointment checked-in, stock level below reorder).
    *   Record detailed information for each audit event:
        *   Timestamp of the event.
        *   User who performed the action (if applicable).
        *   Action type (e.g., 'Create', 'Update', 'Delete', 'Login', 'View').
        *   Data entity affected (e.g., 'Patient', 'Invoice', 'Product').
        *   Record ID of the affected entity (e.g., patient ID, invoice ID).
        *   *Potentially capture 'before' and 'after' state of data for update operations (consider storage implications and if full diff or just key fields are needed).*
        *   Source IP address or location (if relevant).
        *   Event status (success, failure).

*   **Audit Log Storage:**
    *   Store audit logs in a dedicated `FactAuditLogs` database table.
    *   Design the audit log table for efficient querying and reporting.
    *   *Consider data retention policies for audit logs (how long to store logs).*
    *   *Consider security measures to protect audit logs from unauthorized access and modification (tamper-proof logs if required for compliance).*

*   **Audit Log Retrieval and Reporting:**
    *   API endpoints to query and retrieve audit logs based on various criteria (user, date range, action type, entity type, etc.).
    *   Support pagination and filtering for efficient log browsing.
    *   Generate basic audit reports (e.g., audit events by user, audit events by date range, audit events for a specific entity type).
    *   Export audit logs in common formats (e.g., CSV, JSON) for external analysis.

**Database Tables (Relevant Entities):**

*   **FactAuditLogs**: Stores audit log records.
*   **CentralUsers** (or TenantUsers): To link audit events to users.
*   **LookupAuditActionTypes**:  Defines types of audit actions (e.g., 'Create', 'Update', 'Delete', 'Login').
*   **LookupAuditEntityTypes**: Defines types of entities being audited (e.g., 'Patient', 'Invoice', 'Product', 'User').

**Instructions for AI Agent:**

*   Based on this overview and the database schema defined in `10c_Database_Tenant_Fact_Schema.md` and `10e_Database_Tenant_Lookup_Schema.md`, and potentially new lookup tables for audit log specific types, proceed to create the Backend API definitions in `92_AuditLogging_Backend_API.md`.
*   Focus on creating API endpoints for:
    *   Querying and retrieving audit logs (with filtering, pagination, sorting).
    *   Potentially endpoints for exporting audit logs (future enhancement - initially focus on retrieval API).
    *   *No CRUD endpoints for audit logs themselves - audit logs should be created automatically by the system and not manually modifiable or deletable via API by users (except potentially for super-admin for extreme cases, but generally avoid manual modification).*
*   Design a mechanism to automatically generate and record audit logs from backend services and API endpoints in other feature modules (Patient, Appointment, Billing, Inventory, RBAC, etc.).  This could involve:
    *   Creating an `AuditLogService` that provides methods for logging different types of audit events.
    *   Integrating this `AuditLogService` into relevant service methods and API controllers in other modules to call logging methods whenever an auditable action occurs.  This can be done using decorators, interceptors, or by manually calling logging methods within service logic. *Start with manual calling of logging methods within services for simplicity, interceptors or decorators can be considered later.*
*   Ensure API design follows RESTful principles, uses appropriate HTTP methods and status codes, and includes authentication (for audit log retrieval API - restrict to Admin roles).
*   After defining the API, generate the corresponding NestJS backend services in `93_AuditLogging_Backend_Services.md`. These services will handle audit log retrieval and storage logic.