{# 00_Setup_Control_Center.md

**Purpose:**
This file acts as the central control and orchestration point for setting up the foundational structure of the Veterinary PMS project. It will instruct the AI Agent to execute the subsequent setup files in the correct order to establish the backend, frontend, database, and core configurations.

**Instructions:**

1.  **Execute Setup Files in Order:**  The AI agent must execute the following .md files in the exact order listed below. Do not proceed to the next file until the current file's instructions are fully and successfully completed.

    **Execution Plan:**

**Execution Plan:**

**1. Backend Setup:**
    * `setup/01_Backend_Setup_NestJS.md`
    * `setup/02_Backend_Setup_PostgreSQL.md`
    * `setup/03_Backend_Setup_Prisma.md`
    * `setup/04_Backend_Setup_Authentication.md`
    * `setup/05_Backend_Setup_MultiTenancy.md`

**2. Frontend Setup:**
    * `setup/06_Frontend_Setup_NextJS.md`
    * `setup/07_Frontend_Setup_UI_Libraries.md`  *(Updated file)*
    * `setup/08_Frontend_Setup_Routing.md`
    * `setup/09_Frontend_Setup_API_Client.md`

**3. Database Schema Definition:**
    * `db/10_Database_Schema_Overview.md` (for general instructions)
    * `db/10a_Database_Central_Schema.md`
    * `db/10b_Database_Tenant_Dimension_Schema.md`
    * `db/10c_Database_Tenant_Fact_Schema.md`
    * `db/10d_Database_Tenant_Transactional_Schema.md`
    * `db/10e_Database_Tenant_Lookup_Schema.md`
    * `db/10k_Database_Lookup_AuditLog_Schema.md`
    * `db/10g_Database_Lookup_Communication_Schema.md`
    * `db/10i_Database_App_Settings_Schema.md`
    * `db/10j_Database_Fact_AuditLog_Schema.md`
    * `db/10h_Database_Fact_Communication_Schema.md`
    * `db/10l_Database_Tenant_ApiKey_Schema.md`


**4. Frontend Features:**
    * `FrontendFeatures/120_FrontendFeatures_Overview.md` *(New file)*

**5. Backend Features:**
    * **API Key Management:**
        * `API Key Management/110_ApiKeyManagement_Overview.md`
        * `API Key Management/112_ApiKeyManagement_Backend_API.md`
        * `API Key Management/113_ApiKeyManagement_Backend_Services.md`

    * **Appointment Scheduling and Patient Visits:**
        * `Appointment Scheduling and Patient Visits/30_AppointmentVisits_Overview.md`
        * `Appointment Scheduling and Patient Visits/32_AppointmentVisits_Backend_API.md`
        * `Appointment Scheduling and Patient Visits/33_AppointmentVisits_Backend_Services.md`

    * **Audit Logging:**
        * `AuditLogging/90_AuditLogging_Overview.md`
        * `AuditLogging/92_AuditLogging_Backend_API.md`
        * `AuditLogging/93_AuditLogging_Backend_Services.md`

    * **Billing and Invoicing:**
        * `Billing/40_BillingInvoicing_Overview.md`
        * `Billing/42_BillingInvoicing_Backend_API.md`
        * `Billing/43_BillingInvoicing_Backend_Services.md`

    * **Communication Tools (Reminders and Notifications):**
        * `Communication/80_CommunicationTools_Overview.md`
        * `Communication/82_CommunicationTools_Backend_API.md`
        * `Communication/83_CommunicationTools_Backend_Services.md`

    * **Inventory Management:**
        * `Inventory/50_InventoryManagement_Overview.md`
        * `Inventory/52_InventoryManagement_Backend_API.md`
        * `Inventory/53_InventoryManagement_Backend_Services.md`

    * **Patient and Client Management:**
        * `Patient and Client Management/20_PatientManagement_Overview.md`
        * `Patient and Client Management/22_PatientManagement_Backend_API.md`
        * `Patient and Client Management/23_PatientManagement_Backend_Services.md`

    * **Reporting and Analytics:**
        * `Reporting/60_ReportingAnalytics_Overview.md`
        * `Reporting/62_ReportingAnalytics_Backend_API.md`
        * `Reporting/63_ReportingAnalytics_Backend_Services.md`

    * **RBAC (User Roles and Permissions):**
        * `RBAC/70_UserRolesPermissions_Overview.md`
        * `RBAC/72_UserRolesPermissions_Backend_API.md`
        * `RBAC/73_UserRolesPermissions_Backend_Services.md`

    * **User Profile Management:**
        * `User Profile Management/100_UserProfileManagement_Overview.md`
        * `User Profile Management/102_UserProfileManagement_Backend_API.md`
        * `User Profile Management/103_UserProfileManagement_Backend_Services.md`
    



2.  **Verification after each file:** After the AI agent completes the instructions in each file, it MUST provide a summary of the actions taken and confirmation that the steps were successful.  Wait for explicit confirmation before proceeding to the next file.

3.  **Report any errors:** If any errors occur during the execution of any setup file, immediately stop the process and report the error details. Do not attempt to proceed to the next file until the error is resolved.

**Context:**

*   This project is to build a Multi-Tenant Veterinary Practice Management System (PMS).
*   We are using Cursor's AI Agent to automate the development process.
*   The project is broken down into multiple .md files for better management and performance of the AI Agent.
*   This file controls the execution order of the initial setup files.

**Rules and Constraints:**

*   **Strict Execution Order:**  Adhere strictly to the file execution order specified in the instructions.
*   **One File at a Time:**  Focus on completing one setup file completely before moving to the next.
*   **Report Success/Failure:**  Always provide feedback on the outcome of each file execution.

**Details:**

*   The subsequent files (01-10) contain the specific instructions for each setup step.
*   This control file is purely for orchestration and does not contain any coding instructions itself.

**Next Steps (for AI Agent):**

1.  Start executing `01_Backend_Setup_NestJS.md`.}