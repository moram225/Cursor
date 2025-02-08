# 05_Backend_Setup_MultiTenancy.md

**Purpose:**
This file guides the AI Agent to implement the multi-tenancy setup in the NestJS backend. This is a complex step and will focus on setting up dynamic database connection management for each tenant.  We will not fully implement tenant isolation logic in controllers/services yet, but lay the groundwork for dynamic database connections.

**Instructions:**

1.  **Install `nestjs-config` package (optional, for more structured config - or just use `dotenv` directly for now):**
    *   *(For now, let's stick with just `dotenv` and `.env` for simplicity.  We can consider `nestjs-config` later if needed for more complex configurations.)*

2.  **Create a `TenantService`:**
    *   Inside the `src/tenants` folder, create a service named `tenant.service.ts`:

    ```bash
    nest g service tenants/tenant --no-spec
    ```

    *   Implement the following methods in `tenant.service.ts`:
        *   `createTenantDatabase(tenantId: string)`:  This method will be responsible for dynamically creating a new PostgreSQL database for a new tenant. It should:
            *   Accept a `tenantId` (e.g., tenant name or unique identifier).
            *   Construct a new database name based on the `tenantId` (e.g., `veterinary_pms_tenant_${tenantId}`).
            *   Use PostgreSQL commands (via a library or direct command execution) to create the new database.
            *   Return a success/failure indicator.
        *   `getTenantDatabaseUrl(tenantId: string)`: This method will construct and return the database connection URL for a given `tenantId`. It should:
            *   Accept a `tenantId`.
            *   Construct the database URL string based on the tenant's database name (e.g., using environment variables for host, port, user, and dynamically generated database name).
            *   Return the constructed database URL.

    *   Example `tenant.service.ts` (basic structure - database creation logic needs to be implemented):

    ```typescript  typescript
    // src/tenants/tenant.service.ts
    import { Injectable } from '@nestjs/common';
    import { PrismaClient } from '@prisma/client'; // Import PrismaClient (or your PrismaService)

    @Injectable()
    export class TenantService {

      constructor() { } // Inject PrismaService if you want to use it centrally

      async createTenantDatabase(tenantId: string): Promise<boolean> {
        // TODO: Implement dynamic database creation logic in PostgreSQL
        // Example (pseudocode - needs actual PostgreSQL command execution):
        const databaseName = `veterinary_pms_tenant_${tenantId}`;
        console.log(`Creating database: ${databaseName}`);
        // Execute PostgreSQL command to CREATE DATABASE databaseName
        // ... (Implementation for executing PostgreSQL commands needed - e.g., using 'pg' library or similar) ...

        // For now, just simulate success
        return true;
      }

      getTenantDatabaseUrl(tenantId: string): string {
        const databaseName = `veterinary_pms_tenant_${tenantId}`;
        // Construct database URL based on tenant's database name and default connection details
        // (Read host, port, user from environment variables if needed)
        const databaseUrl = `postgresql://postgres:your_dev_password@localhost:5432/${databaseName}?schema=public`; // Replace password if needed
        return databaseUrl;
      }
    }
    ```
    *   **Important:** The `createTenantDatabase` method currently has a placeholder for database creation.  **Implementing the actual PostgreSQL database creation programmatically within NestJS is a more advanced task and might require using a PostgreSQL client library for Node.js (like `pg`) to execute SQL commands directly.**  For this initial setup, we might leave it as a simulated success or a very basic implementation.  We can refine this in later iterations.

3.  **Create a `TenantModule`:**
    *   Inside the `src/tenants` folder, create a module named `tenant.module.ts`:

    ```bash
    nest g module tenants/tenant --no-spec
    ```
    *   Declare `TenantService` as a provider in `TenantModule` and export it.

    ```typescript  typescript
    // src/tenants/tenant.module.ts
    import { Module } from '@nestjs/common';
    import { TenantService } from './tenant.service';

    @Module({
      providers: [TenantService],
      exports: [TenantService], // Export TenantService
    })
    export class TenantModule {}
    ```

4.  **Import `TenantModule` into `AppModule`:**
    *   Import the `TenantModule` into your `AppModule` (`src/app.module.ts`) to make the `TenantService` available.

    ```typescript  typescript
    // src/app.module.ts
    import { Module } from '@nestjs/common';
    import { AppController } from './app.controller';
    import { AppService } from './app.service';
    import { CommonModule } from './common/common.module';
    import { AuthModule } from './auth/auth.module';
    import { TenantModule } from './tenants/tenant.module'; // Import TenantModule

    @Module({
      imports: [CommonModule, AuthModule, TenantModule], // Add TenantModule to imports
      controllers: [AppController],
      providers: [AppService],
    })
    export class AppModule {}
    ```

**Context:**

*   We are setting up the foundation for multi-tenancy in the backend.
*   This initial setup focuses on dynamic database creation and connection management.
*   Tenant isolation logic within application services and controllers will be implemented in later steps.
*   We are creating a `TenantService` to encapsulate tenant-specific operations.

**Rules and Constraints:**

*   Implement dynamic database creation for new tenants.
*   Create a `TenantService` to manage tenant-related operations.
*   Implement `createTenantDatabase` and `getTenantDatabaseUrl` methods in `TenantService`.
*   For now, focus on *simulating* or providing a basic placeholder implementation for `createTenantDatabase`.  Full PostgreSQL command execution can be refined later.
*   Create a `TenantModule` and import `TenantService`.
*   Import `TenantModule` into `AppModule`.

**Details:**

*   Tenant Module: `src/tenants`
*   Tenant Service: `src/tenants/tenant.service.ts`
*   Database naming convention for tenants: `veterinary_pms_tenant_${tenantId}`

**Verification:**

*   After executing these instructions, the AI Agent should confirm:
    *   Creation of `TenantModule` in `src/tenants` folder.
    *   Creation of `TenantService` in `src/tenants`.
    *   Implementation of `createTenantDatabase` (placeholder implementation is acceptable) and `getTenantDatabaseUrl` methods in `TenantService`.
    *   Import of `TenantModule` into `AppModule`.

**Next Steps (for AI Agent):**

1.  Execute the commands and steps outlined in the "Instructions" section.
2.  Verify each step as described in the "Verification" section.
3.  Provide a summary of actions taken and confirmation of successful Multi-Tenancy setup (basic foundation).
4.  Once confirmed, proceed to the next file: `06_Frontend_Setup_NextJS.md`.
