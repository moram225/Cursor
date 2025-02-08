# 50_InventoryManagement_Overview.md

**Core Feature:** Product and Inventory Management

**Purpose:** This feature provides comprehensive product and inventory management capabilities for the Veterinary PMS. It enables clinic staff to manage product information (medications, supplies, food, etc.), track inventory levels across different locations, manage stock transactions (stock in, stock out, adjustments), and receive alerts for low stock.  This feature integrates with Billing (for product sales) and potentially Patient Visits (for recording product usage).

**User Stories:**

*   As a **Inventory Manager/Technician**, I want to **add new products** to the system, including details like name, category, description, unit price, unit cost, supplier, and reorder levels.
*   As a **Inventory Manager/Technician**, I want to **update product information** as needed (price changes, description updates, etc.).
*   As a **Inventory Manager/Technician**, I want to **track current stock levels** for each product at different clinic locations.
*   As a **Inventory Manager/Technician**, I want to **perform stock-in transactions** when new stock is received from suppliers, recording quantities and batch/expiry information.
*   As a **Inventory Manager/Technician**, I want to **perform stock-out transactions** when products are used in patient visits or sold, automatically decreasing stock levels.
*   As a **Inventory Manager/Technician**, I want to **perform stock adjustments** to correct inventory discrepancies (e.g., due to spoilage, damage, or errors).
*   As a **Inventory Manager/Technician**, I want to **receive alerts when stock levels for a product fall below the reorder level** so I can replenish stock in time.
*   As a **Clinic Administrator**, I want to **generate reports on inventory levels, stock valuation, and stock transaction history** for auditing and management purposes.
*   As a **Receptionist/Billing Staff**, I want to **easily select products for invoicing** during checkout or when creating invoices for patient visits.

**Key Functionalities:**

*   **Product Management:**
    *   Create, Read, Update, and Delete (CRUD) product records.
    *   Categorize products (using `LookupProductCategories`).
    *   Record product details: name, description, unit price, unit cost, unit of measure, supplier, reorder level, reorder quantity, active status, inventory tracking flag.
    *   Manage product suppliers (`DimSuppliers`).

*   **Inventory Tracking:**
    *   Track stock levels for each product at different locations (`DimLocations`).
    *   Record stock transactions: Stock In, Stock Out, Stock Adjustment (`FactStockTransactions`).
    *   Manage batch numbers and expiry dates for products where applicable.
    *   Calculate current stock levels based on stock transactions.
    *   Implement low stock alerts based on reorder levels.

*   **Inventory Reporting:**
    *   View current stock levels for all products or specific products.
    *   Generate stock valuation reports (based on unit cost).
    *   Generate stock transaction history reports (for audit trail).
    *   Potentially forecast stock needs based on usage history (future enhancement).

**Database Tables (Relevant Entities):**

*   **DimProducts**: Stores product information.
*   **FactStockTransactions**: Records stock movements (in, out, adjustments).
*   **DimLocations**:  For tracking stock at different locations.
*   **DimSuppliers**: To link products to suppliers.
*   **LookupProductCategories**: For product categorization.
*   **OrderLines & Orders**:  For managing purchase orders and linking to stock in transactions (future linking).

**Instructions for AI Agent:**

*   Based on this overview and the database schema defined in `10b_Database_Tenant_Dimension_Schema.md`, `10c_Database_Tenant_Fact_Schema.md`, and `10e_Database_Tenant_Lookup_Schema.md`, proceed to create the Backend API definitions in `52_InventoryManagement_Backend_API.md`.
*   Focus on creating API endpoints for CRUD operations for `Product` and `StockTransaction` entities, as well as retrieving current stock levels, performing stock transactions (in, out, adjust), and generating basic inventory reports (stock level listing).
*   Ensure API design follows RESTful principles, uses appropriate HTTP methods and status codes, and includes authentication and basic validation.
*   After defining the API, generate the corresponding NestJS backend services in `53_InventoryManagement_Backend_Services.md`.