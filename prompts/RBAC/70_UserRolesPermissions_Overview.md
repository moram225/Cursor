# 70_UserRolesPermissions_Overview.md

**Core Feature:** User Roles and Permissions (RBAC)

**Purpose:** This feature implements Role-Based Access Control (RBAC) within the Veterinary PMS. It defines different user roles with specific permissions, controlling access to various functionalities and data within the application.  RBAC is essential for security, data protection, and ensuring appropriate user access levels based on their responsibilities within the clinic. This feature will be integrated across all backend API endpoints and potentially frontend components to enforce access control.

**User Stories:**

*   As a **Clinic Administrator**, I want to **define different user roles** (e.g., Veterinarian, Receptionist, Technician, Inventory Manager) with varying levels of access to the system.
*   As a **Clinic Administrator**, I want to **assign specific permissions to each role**, determining what actions users in that role can perform (e.g., create appointments, view patient records, generate invoices, manage inventory).
*   As a **Clinic Administrator**, I want to **assign users to roles** so that their access to the system is governed by the permissions of their assigned role.
*   As a **Clinic Administrator**, I want to be able to **modify roles and permissions** as clinic needs and staff responsibilities change.
*   As a **Receptionist**, I should **only be able to access functionalities related to appointment scheduling, client and patient management, and basic billing tasks**, but not sensitive areas like financial reports or inventory management.
*   As a **Veterinarian**, I should be able to **access patient records, appointment schedules, and record visit details**, but not manage system settings or user accounts.
*   As an **Inventory Manager**, I should be able to **access inventory management functionalities**, but not patient medical records or billing information.
*   As a **Super Admin (Central Admin)**, I should have **full access to manage all aspects of the central system and tenant databases**, including user roles, permissions, and system settings.

**Key Functionalities:**

*   **Role Definition:**
    *   Create, Read, Update, and Delete (CRUD) user roles.
    *   Define role names and descriptions.
    *   Assign a set of permissions to each role.

*   **Permission Management:**
    *   Define granular permissions for different actions and data entities (e.g., `create:patient`, `read:patient`, `update:patient`, `delete:patient`, `read:invoice`, `create:appointment`, `manage:inventory`).
    *   Group permissions logically.
    *   Potentially manage permissions in a hierarchical structure.

*   **User-Role Assignment:**
    *   Assign roles to users.
    *   Users can have multiple roles (if needed, but start with single role per user initially for simplicity).
    *   Retrieve user roles and permissions for authentication and authorization.

*   **Access Control Enforcement:**
    *   Implement middleware/guards in the backend API to enforce RBAC based on user roles and permissions.
    *   Potentially implement frontend-level access control to hide/disable UI elements based on user permissions (future enhancement, start with backend enforcement).

**Database Tables (Relevant Entities):**

*   **LookupRoles**: Stores definitions of user roles (role name, description).
*   **LookupPermissions**: Stores definitions of permissions (permission name, description).
*   **RolePermissions**: Many-to-many relationship table linking Roles to Permissions.
*   **CentralUsers** (or potentially TenantUsers table in Tenant DB later if needed):  To link users to roles.  For now, focus on `CentralUsers` and SuperAdmin roles, tenant-level roles can be considered later.

**Instructions for AI Agent:**

*   Based on this overview and the database schema defined in `10e_Database_Tenant_Lookup_Schema.md` and potentially adding new tables as needed, proceed to create the Backend API definitions in `72_UserRolesPermissions_Backend_API.md`.
*   Focus on creating API endpoints for CRUD operations for `Role` and `Permission` entities, as well as managing role-permission assignments.
*   Define API endpoints for assigning roles to users (initially focusing on `CentralUsers`).
*   Design authentication middleware/guards that will use user roles and permissions to authorize access to API endpoints defined in previous feature modules (Patient Management, Appointments, Billing, Inventory, Reports).  This will likely involve JWT authentication (already in place) and extending it to include role/permission information in the JWT or retrieving it from the database based on user identity.
*   Ensure API design follows RESTful principles, uses appropriate HTTP methods and status codes, and includes authentication and validation.
*   After defining the API, generate the corresponding NestJS backend services in `73_UserRolesPermissions_Backend_Services.md`. These services will handle RBAC logic and data access.