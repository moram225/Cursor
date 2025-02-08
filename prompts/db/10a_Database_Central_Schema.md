# 10a_Database_Central_Schema.md

**Purpose:** Defines the database schema for the Central Database, which manages tenants and super admin users. This database is separate from the tenant-specific databases.

**Instructions for AI Agent:**

*   **Generate Prisma schema definitions** for the tables described below.
*   Ensure accurate data types, constraints, and comments based on the descriptions.
*   Note that this schema is for the *Central Database*, distinct from the Tenant Databases.

---

## I. Central Database (Super Admin Management)

This database is for managing tenants and super admin users. It is separate from the tenant-specific databases.

**Tables:**

### 1. Tenants Table (`Tenants`)

*   **tenant_id (PK, UUID, Auto-generated)**: Unique identifier for each tenant (veterinary clinic). Using UUID for security and scalability in a multi-tenant environment.
*   **tenant_name (VARCHAR(255), Not Null)**: Name of the veterinary clinic.
*   **database_name (VARCHAR(255), Unique, Not Null)**: Name of the dedicated PostgreSQL database for this tenant. This must be unique.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the tenant was created.
*   **tenant_admin_email (VARCHAR(255), Not Null)**: Email of the initial tenant administrator. Used during tenant creation.
*   **tenant_admin_password_hash (TEXT, Not Null)**: Hashed password for the initial tenant administrator.
*   **tenant_status (ENUM('Active', 'Inactive', 'Pending'), Default: 'Pending')**: Status of the tenant.
*   **subscription_plan (VARCHAR(255), Null)**: Optional: If you have subscription plans, you can track them here.
*   **contact_phone (VARCHAR(20), Null)**: Clinic's contact phone number.
*   **contact_address (TEXT, Null)**: Clinic's address.
*   **custom_domain (VARCHAR(255), Null)**: Optional: If you offer custom domains.

### 2. CentralUsers Table (`CentralUsers`)

*   **central_user_id (PK, UUID, Auto-generated)**: Unique identifier for super admin users.
*   **email (VARCHAR(255), Unique, Not Null)**: Email address for login.
*   **password_hash (TEXT, Not Null)**: Hashed password for the user.
*   **role (ENUM('SuperAdmin'), Default: 'SuperAdmin')**: Role of the user (currently only SuperAdmin).
*   **first_name (VARCHAR(255), Null)**: First name of the super admin.
*   **last_name (VARCHAR(255), Null)**: Last name of the super admin.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the user was created.
*   **last_login (TIMESTAMP WITH TIME ZONE, Null)**: Timestamp of the user's last login.

### 3. CentralSessions Table (`CentralSessions`)

*   **session_id (PK, UUID, Auto-generated)**: Unique identifier for each session.
*   **central_user_id (FK, UUID, Not Null, References: `CentralUsers`)**: Foreign key to the `CentralUsers` table.
*   **jwt_token (TEXT, Not Null)**: The JWT token.
*   **expiry_at (TIMESTAMP WITH TIME ZONE, Not Null)**: Token expiry timestamp.
*   **login_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the session started.
*   **user_agent (TEXT, Null)**: User agent string from the login request (for security/auditing).
*   **ip_address (INET, Null)**: IP address from the login request (for security/auditing).

---