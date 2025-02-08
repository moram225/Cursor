# 10c_Database_Tenant_Fact_Schema.md

**Purpose:** Defines the schema for Fact Tables within the Tenant Database (per clinic). These tables store transactional data and numerical measures (facts) and are part of the Star Schema design.

**Instructions for AI Agent:**

*   **Generate Prisma schema definitions** for the Fact Tables described below.
*   Ensure accurate data types, constraints, and comments based on the descriptions.
*   Pay close attention to foreign key relationships linking to Dimension Tables and other Transactional tables.
*   Note that these tables belong to the *Tenant Database* and are Facts in a Star Schema.

---

## II. Tenant Database (Per Clinic - Star Schema with 3NF) - B. Fact Tables

Fact tables contain the transactional data or measured events. They link to dimension tables through foreign keys to provide context for the facts.

#### 1. Patient Visits Fact Table (`FactPatientVisits`)

*   **patient_visit_id (PK, UUID, Auto-generated)**: Unique identifier for each patient visit.
*   **patient_id (FK, UUID, Not Null, References: `DimPatients`)**: Foreign key to the `DimPatients` table.
*   **client_id (FK, UUID, Not Null, References: `DimClients`)**: Foreign key to the `DimClients` table (for reporting and data integrity even if patient-client link exists).
*   **staff_id (FK, UUID, Null, References: `DimStaff`)**: Foreign key to the `DimStaff` table - Staff member who attended the visit.
*   **appointment_id (FK, UUID, Null, References: `Appointments`)**: Foreign key to the `Appointments` table (if the visit was scheduled).
*   **location_id (FK, UUID, Null, References: `DimLocations`)**: Foreign key to the `DimLocations` table - Location where the visit took place.
*   **visit_date (DATE, Not Null)**: Date of the patient visit.
*   **visit_start_time (TIME WITH TIME ZONE, Null)**: Start time of the visit.
*   **visit_end_time (TIME WITH TIME ZONE, Null)**: End time of the visit.
*   **reason_for_visit (TEXT, Null)**: Reason for the visit (chief complaint).
*   **diagnosis (TEXT, Null)**: Diagnosis made during the visit.
*   **treatment_notes (TEXT, Null)**: Notes on treatments administered.
*   **vaccinations_administered (TEXT, Null)**: Summary of vaccinations given during the visit. Consider linking to a separate `VisitVaccinations` table for detailed tracking if needed.
*   **medications_prescribed (TEXT, Null)**: Summary of medications prescribed. Consider linking to a separate `VisitMedications` table for detailed tracking if needed.
*   **lab_results_summary (TEXT, Null)**: Summary of lab results from the visit. Link to `LabResults` table for detailed results.
*   **weight (DECIMAL(10, 3), Null)**: Patient's weight during the visit.
*   **temperature (DECIMAL(5, 2), Null)**: Patient's temperature during the visit.
*   **billing_status (ENUM('Unbilled', 'Billed', 'Paid', 'Partially Paid'), Default: 'Unbilled')**: Status of billing for this visit.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the visit record was created.
*   **updated_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the visit record was last updated.

#### 2. Invoice Lines Fact Table (`FactInvoiceLines`)

*   **invoice_line_id (PK, UUID, Auto-generated)**: Unique identifier for each invoice line item.
*   **invoice_id (FK, UUID, Not Null, References: `Invoices`)**: Foreign key to the `Invoices` table.
*   **product_id (FK, UUID, Null, References: `DimProducts`)**: Foreign key to `DimProducts` if the line item is a product. Mutually exclusive with `service_id`.
*   **service_id (FK, UUID, Null, References: `DimServices`)**: Foreign key to `DimServices` if the line item is a service. Mutually exclusive with `product_id`.
*   **description (TEXT, Null)**: Description of the line item (especially useful if ad-hoc or not directly linked to product/service).
*   **quantity (DECIMAL(10, 2), Not Null, Default: 1)**: Quantity of the product or service on this line.
*   **unit_price (DECIMAL(10, 2), Not Null)**: Unit price at the time of invoicing (important to store price at invoice time, in case prices change).
*   **line_total (DECIMAL(10, 2), Not Null, Generated Column as `quantity * unit_price`)**: Calculated line total. Using a generated column for data integrity.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the line item was created.
*   **updated_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the line item was last updated.

#### 3. Payments Fact Table (`FactPayments`)

*   **payment_id (PK, UUID, Auto-generated)**: Unique identifier for each payment.
*   **invoice_id (FK, UUID, Not Null, References: `Invoices`)**: Foreign key to the `Invoices` table.
*   **payment_method_id (FK, UUID, Null, References: `DimPaymentMethods`)**: Foreign key to the `DimPaymentMethods` table.
*   **payment_date (DATE, Not Null)**: Date the payment was received.
*   **payment_amount (DECIMAL(10, 2), Not Null)**: Amount of the payment.
*   **transaction_reference (VARCHAR(255), Null)**: Reference number from the payment system (e.g., card transaction ID).
*   **notes (TEXT, Null)**: Notes about the payment.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the payment record was created.
*   **updated_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the payment record was last updated.

#### 4. Expenses Fact Table (`FactExpenses`)

*   **expense_id (PK, UUID, Auto-generated)**: Unique identifier for each expense record.
*   **expense_category_id (FK, UUID, Not Null, References: `DimExpenseCategories`)**: Foreign key to the `DimExpenseCategories` table.
*   **location_id (FK, UUID, Null, References: `DimLocations`)**: Foreign key to `DimLocations` if the expense is location-specific.
*   **expense_date (DATE, Not Null)**: Date the expense was incurred.
*   **amount (DECIMAL(10, 2), Not Null)**: Amount of the expense.
*   **description (TEXT, Null)**: Description of the expense.
*   **payment_method_id (FK, UUID, Null, References: `DimPaymentMethods`)**: Foreign key to the `DimPaymentMethods` if payment method is tracked for expenses.
*   **supplier_id (FK, UUID, Null, References: `DimSuppliers`)**: Foreign key to the `DimSuppliers` if the expense is related to a supplier invoice.
*   **invoice_reference (VARCHAR(255), Null)**: Reference to supplier invoice number if applicable.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the expense record was created.
*   **updated_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the expense record was last updated.

#### 5. Stock Transactions Fact Table (`FactStockTransactions`)

*   **stock_transaction_id (PK, UUID, Auto-generated)**: Unique identifier for each stock transaction.
*   **product_id (FK, UUID, Not Null, References: `DimProducts`)**: Foreign key to the `DimProducts` table.
*   **location_id (FK, UUID, Not Null, References: `DimLocations`)**: Foreign key to the `DimLocations` table - Location where stock is affected.
*   **transaction_date (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp of the stock transaction.
*   **transaction_type (ENUM('In', 'Out', 'Adjustment'), Not Null)**: Type of transaction (Stock In, Stock Out, Stock Adjustment).
*   **quantity_change (INTEGER, Not Null)**: Change in stock quantity (positive for 'In', negative for 'Out').
*   **batch_number (VARCHAR(255), Null)**: Batch number if applicable.
*   **expiry_date (DATE, Null)**: Expiry date if applicable (especially for medications/vaccines).
*   **reference_document (VARCHAR(255), Null)**: Reference to related document (e.g., Purchase Order number, Internal Stock Transfer number).
*   **notes (TEXT, Null)**: Notes about the stock transaction.
*   **staff_id (FK, UUID, Null, References: `DimStaff`)**: Staff member who performed the stock transaction.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the stock transaction was created.
*   **updated_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the stock transaction was last updated.

#### 6. Order Lines Fact Table (`FactOrderLines`)

*   **order_line_id (PK, UUID, Auto-generated)**: Unique identifier for each order line item.
*   **order_id (FK, UUID, Not Null, References: `Orders`)**: Foreign key to the `Orders` table.
*   **product_id (FK, UUID, Not Null, References: `DimProducts`)**: Foreign key to the `DimProducts` table.
*   **quantity_ordered (INTEGER, Not Null)**: Quantity ordered.
*   **unit_price (DECIMAL(10, 2), Not Null)**: Unit price at the time of ordering.
*   **line_total (DECIMAL(10, 2), Not Null, Generated Column as `quantity_ordered * unit_price`)**: Calculated line total.
*   **quantity_received (INTEGER, Default: 0)**: Quantity received (tracks partial order fulfillment).
*   **notes (TEXT, Null)**: Notes about the order line.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the order line was created.
*   **updated_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the order line was last updated.