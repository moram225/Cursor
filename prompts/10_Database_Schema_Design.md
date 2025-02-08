# 10_Database_Schema_Design.md

**Purpose:**
This file is crucial for defining the database schema for the Veterinary PMS. It will detail the tables, columns, data types, relationships, and constraints. The schema will be designed considering both 3rd Normal Form (3NF) for transactional data integrity and elements of a Star Schema to facilitate efficient reporting and analytics.

**Instructions:**

1.  **Define 3NF (Transactional) Tables:**

    *   **Tenant Table:**
        *   TableName: `Tenants`
        *   Columns:
            *   `tenant_id` (UUID, Primary Key, Unique, Not Null): Unique identifier for each tenant (veterinary clinic).
            *   `tenant_name` (VARCHAR(255), Not Null): Name of the veterinary clinic.
            *   `contact_email` (VARCHAR(255)): Clinic's contact email.
            *   `contact_phone` (VARCHAR(20)): Clinic's contact phone number.
            *   `address_line1` (VARCHAR(255)): Clinic's address line 1.
            *   `address_line2` (VARCHAR(255)): Clinic's address line 2 (optional).
            *   `city` (VARCHAR(100)): Clinic's city.
            *   `state` (VARCHAR(100)): Clinic's state/region.
            *   `postal_code` (VARCHAR(20)): Clinic's postal code.
            *   `country` (VARCHAR(100)): Clinic's country.
            *   `created_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of tenant creation.
            *   `updated_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of last update.

    *   **Location Table:** (For practices with multiple locations)
        *   TableName: `Locations`
        *   Columns:
            *   `location_id` (UUID, Primary Key, Unique, Not Null): Unique identifier for each location within a tenant.
            *   `tenant_id` (UUID, Foreign Key referencing `Tenants.tenant_id`, Not Null): Tenant to which this location belongs.
            *   `location_name` (VARCHAR(255), Not Null): Name of the location (e.g., "Main Clinic", "Branch 1").
            *   `address_line1` (VARCHAR(255)): Location address line 1.
            *   `address_line2` (VARCHAR(255)): Location address line 2 (optional).
            *   `city` (VARCHAR(100)): Location city.
            *   `state` (VARCHAR(100)): Location state/region.
            *   `postal_code` (VARCHAR(20)): Location postal code.
            *   `country` (VARCHAR(100)): Location country.
            *   `contact_phone` (VARCHAR(20)): Location contact phone.
            *   `created_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of location creation.
            *   `updated_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of last update.

    *   **User Table:** (Clinic Staff Users)
        *   TableName: `Users`
        *   Columns:
            *   `user_id` (UUID, Primary Key, Unique, Not Null): Unique identifier for each user.
            *   `tenant_id` (UUID, Foreign Key referencing `Tenants.tenant_id`, Not Null): Tenant to which the user belongs.
            *   `location_id` (UUID, Foreign Key referencing `Locations.location_id`, Nullable):  Location to which the user is primarily assigned (can be null if not location-specific).
            *   `email` (VARCHAR(255), Unique, Not Null): User's login email.
            *   `password_hash` (VARCHAR(255), Not Null): Hashed password.
            *   `first_name` (VARCHAR(100), Not Null): User's first name.
            *   `last_name` (VARCHAR(100), Not Null): User's last name.
            *   `role` (VARCHAR(50), Not Null, Default: 'user', Enum: ['superadmin', 'admin', 'user']): User's role (RBAC).
            *   `created_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of user creation.
            *   `updated_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of last update.

    *   **Client Table:** (Pet Owners)
        *   TableName: `Clients`
        *   Columns:
            *   `client_id` (UUID, Primary Key, Unique, Not Null): Unique identifier for each client.
            *   `tenant_id` (UUID, Foreign Key referencing `Tenants.tenant_id`, Not Null): Tenant to which the client belongs.
            *   `first_name` (VARCHAR(100), Not Null): Client's first name.
            *   `last_name` (VARCHAR(100), Not Null): Client's last name.
            *   `primary_phone` (VARCHAR(20), Not Null): Client's primary phone number.
            *   `secondary_phone` (VARCHAR(20)): Client's secondary phone number (optional).
            *   `email` (VARCHAR(255)): Client's email address (optional).
            *   `address_line1` (VARCHAR(255)): Client's address line 1.
            *   `address_line2` (VARCHAR(255)): Client's address line 2 (optional).
            *   `city` (VARCHAR(100)): Client's city.
            *   `state` (VARCHAR(100)): Client's state/region.
            *   `postal_code` (VARCHAR(20)): Client's postal code.
            *   `country` (VARCHAR(100)): Client's country.
            *   `notes` (TEXT):  Notes about the client.
            *   `created_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of client creation.
            *   `updated_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of last update.

    *   **Patient Table:** (Pets)
        *   TableName: `Patients`
        *   Columns:
            *   `patient_id` (UUID, Primary Key, Unique, Not Null): Unique identifier for each patient.
            *   `tenant_id` (UUID, Foreign Key referencing `Tenants.tenant_id`, Not Null): Tenant to which the patient record belongs.
            *   `client_id` (UUID, Foreign Key referencing `Clients.client_id`, Not Null): Owner of the patient.
            *   `patient_name` (VARCHAR(100), Not Null): Patient's name.
            *   `animal_type` (VARCHAR(100), Not Null): Type of animal (e.g., "Dog", "Cat", "Bird").
            *   `breed` (VARCHAR(100)): Patient's breed (optional).
            *   `date_of_birth` (DATE): Patient's date of birth (optional).
            *   `sex` (VARCHAR(20), Enum: ['Male', 'Female', 'Unknown']): Patient's sex.
            *   `color` (VARCHAR(100)): Patient's color (optional).
            *   `weight_kg` (DECIMAL): Patient's weight in kilograms (optional).
            *   `microchip_number` (VARCHAR(100)): Patient's microchip number (optional).
            *   `medical_history_notes` (TEXT):  General medical history notes.
            *   `created_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of patient creation.
            *   `updated_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of last update.

    *   **Appointment Table:**
        *   TableName: `Appointments`
        *   Columns:
            *   `appointment_id` (UUID, Primary Key, Unique, Not Null): Unique identifier for each appointment.
            *   `tenant_id` (UUID, Foreign Key referencing `Tenants.tenant_id`, Not Null): Tenant to which the appointment belongs.
            *   `location_id` (UUID, Foreign Key referencing `Locations.location_id`, Not Null): Location where the appointment is scheduled.
            *   `patient_id` (UUID, Foreign Key referencing `Patients.patient_id`, Not Null): Patient for the appointment.
            *   `client_id` (UUID, Foreign Key referencing `Clients.client_id`, Not Null): Client for the appointment.
            *   `vet_user_id` (UUID, Foreign Key referencing `Users.user_id`, Nullable): Veterinarian assigned to the appointment (optional - can be assigned later).
            *   `appointment_type` (VARCHAR(100), Not Null): Type of appointment (e.g., "Consultation", "Vaccination", "Surgery").
            *   `start_time` (TIMESTAMP WITH TIME ZONE, Not Null): Appointment start time.
            *   `end_time` (TIMESTAMP WITH TIME ZONE, Not Null): Appointment end time.
            *   `status` (VARCHAR(50), Not Null, Default: 'scheduled', Enum: ['scheduled', 'confirmed', 'completed', 'cancelled']): Appointment status.
            *   `notes` (TEXT): Notes about the appointment.
            *   `created_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of appointment creation.
            *   `updated_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of last update.

    *   **Invoice Table:**
        *   TableName: `Invoices`
        *   Columns:
            *   `invoice_id` (UUID, Primary Key, Unique, Not Null): Unique identifier for each invoice.
            *   `tenant_id` (UUID, Foreign Key referencing `Tenants.tenant_id`, Not Null): Tenant to which the invoice belongs.
            *   `client_id` (UUID, Foreign Key referencing `Clients.client_id`, Not Null): Client for the invoice.
            *   `appointment_id` (UUID, Foreign Key referencing `Appointments.appointment_id`, Nullable): Appointment related to the invoice (optional - invoice can be independent of appointment).
            *   `invoice_number` (VARCHAR(50), Unique, Not Null):  Unique invoice number (consider a format like INV-YYYYMMDD-XXXX).
            *   `invoice_date` (DATE, Not Null): Date of invoice generation.
            *   `due_date` (DATE): Invoice due date (optional).
            *   `total_amount` (DECIMAL, Not Null): Total amount of the invoice.
            *   `amount_paid` (DECIMAL, Default: 0.00, Not Null): Total amount paid against the invoice.
            *   `payment_status` (VARCHAR(50), Not Null, Default: 'pending', Enum: ['pending', 'paid', 'partially_paid', 'overdue', 'cancelled']): Payment status of the invoice.
            *   `notes` (TEXT): Invoice notes or description.
            *   `created_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of invoice creation.
            *   `updated_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of last update.

    *   **Invoice Item Table:** (Line items for each invoice)
        *   TableName: `InvoiceItems`
        *   Columns:
            *   `invoice_item_id` (UUID, Primary Key, Unique, Not Null): Unique identifier for each invoice item.
            *   `invoice_id` (UUID, Foreign Key referencing `Invoices.invoice_id`, Not Null): Invoice to which this item belongs.
            *   `item_description` (VARCHAR(255), Not Null): Description of the item/service.
            *   `quantity` (DECIMAL, Not Null, Default: 1): Quantity of the item/service.
            *   `unit_price` (DECIMAL, Not Null): Price per unit.
            *   `total_price` (DECIMAL, Not Null): Total price of the line item (calculated as quantity * unit_price).

    *   **Payment Table:** (Records of payments made)
        *   TableName: `Payments`
        *   Columns:
            *   `payment_id` (UUID, Primary Key, Unique, Not Null): Unique identifier for each payment record.
            *   `tenant_id` (UUID, Foreign Key referencing `Tenants.tenant_id`, Not Null): Tenant to which the payment belongs.
            *   `invoice_id` (UUID, Foreign Key referencing `Invoices.invoice_id`, Not Null): Invoice to which this payment is applied.
            *   `payment_date` (DATE, Not Null): Date of payment.
            *   `payment_method` (VARCHAR(50), Not Null, Enum: ['cash', 'card', 'check', 'online_transfer', 'other']): Method of payment.
            *   `payment_amount` (DECIMAL, Not Null): Amount paid.
            *   `transaction_reference` (VARCHAR(255)): Transaction reference number (optional).
            *   `notes` (TEXT): Payment notes (optional).
            *   `created_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of payment record creation.
            *   `updated_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of last update.

    *   **Inventory Item Table:**
        *   TableName: `InventoryItems`
        *   Columns:
            *   `inventory_item_id` (UUID, Primary Key, Unique, Not Null): Unique identifier for each inventory item.
            *   `tenant_id` (UUID, Foreign Key referencing `Tenants.tenant_id`, Not Null): Tenant to which the inventory item belongs.
            *   `location_id` (UUID, Foreign Key referencing `Locations.location_id`, Not Null): Location where the inventory item is stocked.
            *   `item_name` (VARCHAR(255), Not Null): Name of the inventory item (e.g., "Vaccine X", "Syringe").
            *   `description` (TEXT): Description of the inventory item.
            *   `category` (VARCHAR(100)): Category of the item (e.g., "Vaccine", "Medication", "Supply").
            *   `unit_of_measure` (VARCHAR(50), Not Null, Default: 'unit'): Unit of measurement (e.g., "unit", "ml", "box").
            *   `reorder_level` (DECIMAL):  Quantity at which to reorder.
            *   `expiry_date` (DATE): Expiry date of the item (if applicable).
            *   `batch_number` (VARCHAR(100)): Batch number (if applicable).
            *   `supplier_id` (VARCHAR(255)): Supplier information or link to a Supplier table (optional - can be added later).
            *   `created_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of inventory item creation.
            *   `updated_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of last update.

    *   **Stock Level Table:** (Tracks current stock levels at each location - could be combined with InventoryItems or separate for performance)
        *   TableName: `StockLevels`
        *   Columns:
            *   `stock_level_id` (UUID, Primary Key, Unique, Not Null): Unique identifier for stock level record.
            *   `inventory_item_id` (UUID, Foreign Key referencing `InventoryItems.inventory_item_id`, Not Null): Inventory item for which stock level is tracked.
            *   `location_id` (UUID, Foreign Key referencing `Locations.location_id`, Not Null): Location for which stock level is tracked.
            *   `quantity_on_hand` (DECIMAL, Not Null, Default: 0): Current quantity in stock.
            *   `last_stock_update` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of last stock level update.

    *   **Expense Category Table:**
        *   TableName: `ExpenseCategories`
        *   Columns:
            *   `expense_category_id` (UUID, Primary Key, Unique, Not Null): Unique identifier for expense category.
            *   `tenant_id` (UUID, Foreign Key referencing `Tenants.tenant_id`, Not Null): Tenant for expense category.
            *   `category_name` (VARCHAR(255), Not Null): Name of the expense category (e.g., "Supplies", "Utilities", "Rent", "Salaries").
            *   `description` (TEXT): Description of the expense category.
            *   `created_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of expense category creation.
            *   `updated_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of last update.

    *   **Expense Table:**
        *   TableName: `Expenses`
        *   Columns:
            *   `expense_id` (UUID, Primary Key, Unique, Not Null): Unique identifier for each expense record.
            *   `tenant_id` (UUID, Foreign Key referencing `Tenants.tenant_id`, Not Null): Tenant for expense record.
            *   `location_id` (UUID, Foreign Key referencing `Locations.location_id`, Nullable): Location where expense incurred (optional).
            *   `expense_category_id` (UUID, Foreign Key referencing `ExpenseCategories.expense_category_id`, Not Null): Category of the expense.
            *   `expense_date` (DATE, Not Null): Date of expense.
            *   `amount` (DECIMAL, Not Null): Amount of expense.
            *   `description` (TEXT): Description of the expense.
            *   `payment_method` (VARCHAR(50), Enum: ['cash', 'card', 'check', 'online_transfer', 'other']): Payment method for expense (optional).
            *   `reference_number` (VARCHAR(255)): Reference number for expense (e.g., receipt number).
            *   `created_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of expense record creation.
            *   `updated_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of last update.

    *   **Communication Log Table:**
        *   TableName: `CommunicationLogs`
        *   Columns:
            *   `communication_log_id` (UUID, Primary Key, Unique, Not Null): Unique identifier for communication log.
            *   `tenant_id` (UUID, Foreign Key referencing `Tenants.tenant_id`, Not Null): Tenant for communication log.
            *   `client_id` (UUID, Foreign Key referencing `Clients.client_id`, Nullable): Client involved in communication (optional).
            *   `patient_id` (UUID, Foreign Key referencing `Patients.patient_id`, Nullable): Patient involved in communication (optional).
            *   `communication_type` (VARCHAR(50), Not Null, Enum: ['SMS', 'Email', 'Phone Call']): Type of communication.
            *   `direction` (VARCHAR(50), Not Null, Enum: ['outgoing', 'incoming']): Direction of communication.
            *   `subject` (VARCHAR(255)): Subject of communication (e.g., for emails).
            *   `message_content` (TEXT): Content of the message.
            *   `sent_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp when communication was sent/received.
            *   `status` (VARCHAR(50), Enum: ['pending', 'sent', 'delivered', 'failed', 'received']): Status of communication.
            *   `notes` (TEXT): Additional notes on communication.
            *   `created_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of communication log creation.
            *   `updated_at` (TIMESTAMP WITH TIME ZONE, Default: `CURRENT_TIMESTAMP`, Not Null): Timestamp of last update.

    *   **(Optional 3NF Tables - for later expansion, can be omitted initially):**
        *   Lab Results Table, Vaccination Records Table, Medication Records Table, Patient Medical History (detailed event-based history).  *(We can define these in detail later if needed, for now, `Patients.medical_history_notes` and `Appointments.notes` can serve as basic notes fields).*

2.  **Define Star Schema (Reporting) Elements (Conceptual - Table definitions can be added later if needed):**

    *   **Fact Tables (Examples):**
        *   `FactSales`:  Fact table to track sales revenue.  Facts: `revenue_amount`, `quantity_sold`. Dimensions: `date_dimension_id`, `product_dimension_id`, `service_dimension_id`, `client_dimension_id`, `location_dimension_id`, `staff_dimension_id`, `tenant_id`.
        *   `FactExpenses`: Fact table for expenses. Facts: `expense_amount`. Dimensions: `date_dimension_id`, `expense_category_dimension_id`, `location_dimension_id`, `tenant_id`.
        *   `FactAppointments`: Fact table for appointment metrics. Facts: `appointment_count`. Dimensions: `date_dimension_id`, `appointment_type_dimension_id`, `location_dimension_id`, `vet_dimension_id`, `tenant_id`.

    *   **Dimension Tables (Examples):**
        *   `DateDimension`: `date_dimension_id` (PK), `date`, `month`, `year`, `day_of_week`, etc. (for time-based analysis).
        *   `ProductDimension`: `product_dimension_id` (PK), `product_name`, `category`, etc. (from `InventoryItems`).
        *   `ServiceDimension`: `service_dimension_id` (PK), `service_name`, `service_category`, etc. (service types from `AppointmentTypes` or similar - may need to create a service type lookup table if not already present in appointment types).
        *   `ClientDimension`: `client_dimension_id` (PK), `client_name`, `client_city`, `client_type` (if client types are categorized), etc. (from `Clients`).
        *   `LocationDimension`: `location_dimension_id` (PK), `location_name`, `location_city`, `location_type` (if locations have types), etc. (from `Locations`).
        *   `StaffDimension`: `staff_dimension_id` (PK), `staff_name`, `staff_role`, etc. (from `Users`).
        *   `ExpenseCategoryDimension`: `expense_category_dimension_id` (PK), `category_name`, etc. (from `ExpenseCategories`).
        *   `AppointmentTypeDimension`: `appointment_type_dimension_id` (PK), `appointment_type_name`, etc. (from `Appointments.appointment_type` - or a separate `AppointmentTypes` lookup table if needed).

3.  **Data Types:**
    *   Use appropriate PostgreSQL data types for each column (VARCHAR, TEXT, INTEGER, DECIMAL, DATE, TIMESTAMP WITH TIME ZONE, UUID, ENUM).  Refer to PostgreSQL documentation for data type details.
    *   Use UUID for Primary Keys for most tables for scalability and distributed systems considerations.
    *   Use ENUM types where applicable for constrained values (e.g., `role` in `Users`, `payment_method` in `Payments`, `status` in `Appointments`).

4.  **Relationships & Constraints:**
    *   Clearly define Primary Keys (PK), Foreign Keys (FK), Unique constraints, Not Null constraints as indicated in the table definitions.
    *   Establish foreign key relationships between tables to enforce referential integrity (e.g., `Appointments.patient_id` FK references `Patients.patient_id` PK).
    *   Consider adding indexes to frequently queried columns (especially foreign keys and columns used in WHERE clauses) for performance optimization, but index creation is not explicitly defined in this schema definition file, it's a database optimization step to consider later.

**Context:**

*   This document defines the database schema for the Multi-Tenant Veterinary PMS.
*   The schema is designed to be implemented in PostgreSQL and used with Prisma ORM.
*   It incorporates 3NF principles for transactional tables to minimize data redundancy and ensure data integrity.
*   It conceptually outlines Star Schema elements for reporting and analytics capabilities.  The actual Star Schema table creation can be implemented in later phases.
*   Multi-tenancy is addressed by including `tenant_id` in most transactional tables to isolate data per tenant. `location_id` is used for multi-location support within a tenant.

**Rules and Constraints:**

*   Design a database schema suitable for a Multi-Tenant Veterinary PMS.
*   Adhere to 3NF principles for transactional tables.
*   Incorporate elements of Star Schema for reporting (at least conceptually outlined).
*   Use PostgreSQL data types.
*   Use UUID as Primary Key for most tables.
*   Define clear table and column names, data types, constraints (PK, FK, Not Null, Unique), and relationships.
*   Include `tenant_id` in relevant tables for multi-tenancy.
*   Consider `location_id` for multi-location support.
*   Use ENUM types for constrained value columns.

**Details:**

*   Database System: PostgreSQL
*   ORM: Prisma
*   Primary Key type: UUID
*   Multi-tenancy approach: Database per tenant (initially conceptual - schema supports it). Schema design includes `tenant_id` for data isolation within a shared database approach as well if needed.

**Verification:**

*   After defining the schema, the AI Agent should provide a summary of the defined tables, including:
    *   For each table: Table Name, Purpose/Description.
    *   For each table, list of Columns with: Column Name, Data Type, Constraints (PK, FK, Not Null, Unique, Enum if applicable), and description/purpose.
    *   Summary of Foreign Key relationships between tables.
    *   Outline of Star Schema elements (Fact tables and Dimension tables described conceptually).

**Next Steps (for AI Agent):**

1.  Thoroughly review the entire database schema definition provided in this file.
2.  Summarize the schema as described in the "Verification" section, listing tables, columns, data types, constraints, relationships, and star schema elements. Provide this summary as output.
3.  Once the schema design is reviewed and confirmed, the next step would be to implement this schema in the `prisma/schema.prisma` file in `03_Backend_Setup_Prisma.md` (update the existing `schema.prisma` with these detailed models).  However, as per the control file execution order, the next file to process after setup files is likely to be related to core features.