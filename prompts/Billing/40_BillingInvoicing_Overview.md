# 40_BillingInvoicing_Overview.md

**Core Feature:** Billing and Invoicing

**Purpose:** This feature provides comprehensive billing and invoicing functionality for the Veterinary PMS. It allows clinic staff to create invoices for patient visits and product/service sales, manage payment collection, track invoice statuses, and generate financial reports. This feature directly integrates with Patient Visits and Product/Service Management.

**User Stories:**

*   As a **Receptionist**, I want to **create invoices** for patient visits, automatically including services performed and products used during the visit.
*   As a **Receptionist**, I want to **add or remove line items** on an invoice manually for additional charges or discounts.
*   As a **Receptionist**, I want to **record payments** against invoices, specifying payment method and amount.
*   As a **Receptionist**, I want to **view invoice details**, including line items, total amount, payments received, and balance due.
*   As a **Receptionist/Clinic Administrator**, I want to **track invoice statuses** (Draft, Sent, Paid, Overdue, etc.) to manage outstanding payments.
*   As a **Clinic Administrator**, I want to **generate reports on invoices and payments** for a specific period to track revenue and financial performance.
*   As a **Client**, I want to **receive invoices** (via email or client portal - future enhancement) for services and products.
*   As a **Client**, I want to be able to **make payments online** (future enhancement - initially internal staff management of payments).

**Key Functionalities:**

*   **Invoice Creation:**
    *   Create new invoices linked to clients (and optionally patients/visits).
    *   Automatically generate invoice line items from patient visits (services, products used during visit).
    *   Manually add/edit/remove invoice line items (products, services, ad-hoc charges, discounts).
    *   Set invoice date, due date, and invoice number (with configurable prefixes/sequences).
    *   Manage invoice statuses (Draft, Sent, Paid, Partially Paid, Overdue, Void).
    *   Calculate invoice totals and balance due automatically.

*   **Payment Management:**
    *   Record payments against invoices, specifying payment method, date, and amount.
    *   Track payment history for each invoice.
    *   Automatically update invoice status based on payments received.
    *   Potentially handle partial payments and overpayments.

*   **Invoice Management & Reporting:**
    *   View, search, and filter invoices by various criteria (client, invoice number, date range, status, etc.).
    *   Generate invoice reports (e.g., invoices by date range, outstanding invoices, payment summaries).
    *   Potentially generate and send invoices to clients via email (future enhancement).

**Database Tables (Relevant Entities):**

*   **Invoices**: Stores invoice header information.
*   **FactInvoiceLines**: Stores individual line items for each invoice (linking to products and services).
*   **FactPayments**: Stores payment records linked to invoices.
*   **DimClients**: To link invoices to clients.
*   **DimPaymentMethods**: To record payment methods.
*   **FactPatientVisits**:  Source of services and products for invoice generation (indirect link).

**Instructions for AI Agent:**

*   Based on this overview and the database schema defined in `10d_Database_Tenant_Transactional_Schema.md`, `10c_Database_Tenant_Fact_Schema.md`, and `10e_Database_Tenant_Lookup_Schema.md`, proceed to create the Backend API definitions in `42_BillingInvoicing_Backend_API.md`.
*   Focus on creating API endpoints for CRUD operations for `Invoice` and `Payment` entities, as well as invoice line item management, payment recording, and invoice status tracking.
*   Include endpoints for generating basic invoice reports (listings and summaries - e.g., invoices within a date range, outstanding balances).
*   Ensure API design follows RESTful principles, uses appropriate HTTP methods and status codes, and includes authentication and basic validation.
*   After defining the API, generate the corresponding NestJS backend services in `43_BillingInvoicing_Backend_Services.md`.