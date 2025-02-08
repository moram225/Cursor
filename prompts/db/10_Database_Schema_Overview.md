# 10_Database_Schema_Overview.md

**Purpose:** This set of files defines the complete database schema for the Multi-Tenant Veterinary PMS project, split into logical sections for better management and AI agent processing. It includes schemas for both the central database (for tenant management) and the tenant-specific databases (for clinic data). This schema is designed to be implemented using PostgreSQL and Prisma ORM.

**Instructions for AI Agent:**

*   **Generate Prisma schema files** based on the table definitions provided in the following files:
    *   `10a_Database_Central_Schema.md` (Central Database)
    *   `10b_Database_Tenant_Dimension_Schema.md` (Tenant Database - Dimension Tables)
    *   `10c_Database_Tenant_Fact_Schema.md` (Tenant Database - Fact Tables)
    *   `10d_Database_Tenant_Transactional_Schema.md` (Tenant Database - Transactional Tables)
    *   `10e_Database_Tenant_Lookup_Schema.md` (Tenant Database - Lookup Tables)
*   Ensure that the Prisma schema accurately reflects the data types, constraints (Not Null, Unique, Foreign Keys), and relationships as specified in each file.
*   Pay close attention to the distinction between the Central Database schema and the Tenant Database schema.
*   For the Tenant Database schema files (`10b_Database_Tenant_Dimension_Schema.md`, `10c_Database_Tenant_Fact_Schema.md`, `10d_Database_Tenant_Transactional_Schema.md`, `10e_Database_Tenant_Lookup_Schema.md`), note the star schema design principles and the separation of Dimension, Fact, Transactional, and Lookup tables.
*   Generate comments in the Prisma schema based on the table and column descriptions provided in these documents to enhance readability and maintainability.
*   The order of processing these files might be important due to foreign key relationships. Consider processing them in the order listed above for the Tenant Database (Dimension, then Fact, then Transactional, then Lookup). Central Database can be processed first or alongside Dimension tables as it has no dependencies on Tenant DB tables.

---

## III. 3NF Compliance Notes:

*   **No Redundancy**: Data is not unnecessarily repeated across tables. For example, client details are in `DimClients`, not repeated in `FactPatientVisits` (only linked via `client_id`).
*   **No Repeating Groups**: Tables do not have columns that store multiple values. Line items for invoices are in `FactInvoiceLines`, not as repeating columns in `Invoices`.
*   **No Transitive Dependencies**: Non-key attributes depend only on the primary key. For example, in `DimProducts`, `unit_price` depends on `product_id`, not on another non-key attribute of `DimProducts`.

---

## IV. Star Schema and Performance:

*   **Fact Tables**: `FactPatientVisits`, `FactInvoiceLines`, `FactPayments`, `FactExpenses`, `FactStockTransactions`, `FactOrderLines` are designed to store core business events and numerical measures (facts).
*   **Dimension Tables**: `DimClients`, `DimPatients`, `DimStaff`, `DimProducts`, `DimServices`, `DimSuppliers`, `DimExpenseCategories`, `DimPaymentMethods`, `DimLocations` provide context for the facts, allowing for efficient filtering, grouping, and aggregation in reporting and analytics.
*   **Star Schema Benefits**: This structure simplifies complex queries, improves query performance for reporting, and makes the database easier to understand for analytical purposes.

---

## V. Further Considerations and Enhancements:

*   **Auditing**: For critical tables (especially financial and stock related), consider adding audit trails (trigger-based or separate audit tables) to track changes, who made them, and when.
*   **Data Archiving**: For very large clinics, consider strategies for archiving historical data (e.g., older patient visit records) to maintain performance.
*   **Indexing**: Implement appropriate indexes on frequently queried columns, especially foreign keys in fact tables and columns used in `WHERE` clauses and `JOIN` conditions.
*   **Data Validation**: Implement database-level constraints (`NOT NULL`, `UNIQUE`, `CHECK`) and application-level validation to ensure data quality.
*   **Integrations**: Design integration points (APIs, views, etc.) for lab systems, accounting software, and other external platforms as needed.
*   **Scalability**: PostgreSQL is highly scalable. For extreme scale, you could consider database sharding or read replicas if needed in the future, but the per-tenant database approach is already very scalable.
*   **Security**: Enforce RBAC at the application level using JWT authentication. Ensure proper database user permissions so that tenant databases are isolated and only accessible by authorized users and the application. Regularly review and update security practices.
*   **Backup and Recovery**: Implement robust backup and recovery strategies for both the central database and tenant databases.

---