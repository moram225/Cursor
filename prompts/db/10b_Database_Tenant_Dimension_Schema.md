# 10b_Database_Tenant_Dimension_Schema.md

**Purpose:** Defines the schema for Dimension Tables within the Tenant Database (per clinic). These tables provide descriptive attributes for the Fact Tables and are part of the Star Schema design.

**Instructions for AI Agent:**

*   **Generate Prisma schema definitions** for the Dimension Tables described below.
*   Ensure accurate data types, constraints, and comments based on the descriptions.
*   Note that these tables belong to the *Tenant Database* and are Dimensions in a Star Schema.

---

## II. Tenant Database (Per Clinic - Star Schema with 3NF) - A. Dimension Tables

Dimension tables hold descriptive attributes that provide context for the facts. They are generally denormalized for star schema performance, but in this design, we maintain 3NF within dimensions where practical to avoid obvious redundancies.

#### 1. Clients Dimension (`DimClients`)

*   **client_id (PK, UUID, Auto-generated)**: Unique identifier for each client (pet owner).
*   **client_name (VARCHAR(255), Not Null)**: Full name of the client.
*   **contact_phone (VARCHAR(20), Null)**: Client's primary phone number.
*   **contact_phone_secondary (VARCHAR(20), Null)**: Client's secondary phone number (optional).
*   **email (VARCHAR(255), Null)**: Client's email address.
*   **address_line1 (VARCHAR(255), Null)**: Address line 1.
*   **address_line2 (VARCHAR(255), Null)**: Address line 2 (optional).
*   **city (VARCHAR(255), Null)**: City.
*   **state (VARCHAR(255), Null)**: State/Province.
*   **postal_code (VARCHAR(20), Null)**: Postal/Zip code.
*   **notes (TEXT, Null)**: General notes about the client.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the client record was created.
*   **updated_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the client record was last updated.

#### 2. Patients Dimension (`DimPatients`)

*   **patient_id (PK, UUID, Auto-generated)**: Unique identifier for each patient (animal).
*   **client_id (FK, UUID, Not Null, References: `DimClients`)**: Foreign key to the `DimClients` table.
*   **patient_name (VARCHAR(255), Not Null)**: Name of the patient.
*   **animal_type (VARCHAR(255), Null)**: Species (e.g., Dog, Cat, Bird). Consider a lookup table if you have a fixed list.
*   **breed (VARCHAR(255), Null)**: Breed of the animal. Consider a lookup table if you have a controlled list.
*   **date_of_birth (DATE, Null)**: Patient's date of birth.
*   **gender (ENUM('Male', 'Female', 'Unknown'), Default: 'Unknown')**: Patient's gender.
*   **color (VARCHAR(255), Null)**: Patient's color/markings.
*   **medical_history (TEXT, Null)**: Summary of patient's medical history. Detailed medical events are tracked in `FactPatientVisits`.
*   **vaccination_notes (TEXT, Null)**: Summary of vaccinations. Detailed vaccinations are tracked in `FactPatientVisits` and potentially a separate `PatientVaccinations` table for historical records.
*   **medication_notes (TEXT, Null)**: Summary of current medications. Detailed medications are tracked in `FactPatientVisits` and potentially a separate `PatientMedications` table for historical records.
*   **microchip_number (VARCHAR(255), Unique, Null)**: Microchip number (optional, but unique if present).
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the patient record was created.
*   **updated_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the patient record was last updated.

#### 3. Staff Dimension (`DimStaff`)

*   **staff_id (PK, UUID, Auto-generated)**: Unique identifier for each staff member.
*   **email (VARCHAR(255), Unique, Not Null)**: Staff member's email for login.
*   **password_hash (TEXT, Not Null)**: Hashed password.
*   **role (ENUM('SuperAdmin', 'TenantAdmin', 'Vet', 'Receptionist', 'Technician', 'Other'), Default: 'Other')**: Role of the staff member. `TenantAdmin` role exists within the tenant database for tenant-level administration. `SuperAdmin` is only in the central database.
*   **first_name (VARCHAR(255), Not Null)**: First name.
*   **last_name (VARCHAR(255), Not Null)**: Last name.
*   **contact_phone (VARCHAR(20), Null)**: Contact phone.
*   **address_line1 (VARCHAR(255), Null)**: Address line 1.
*   **address_line2 (VARCHAR(255), Null)**: Address line 2 (optional).
*   **city (VARCHAR(255), Null)**: City.
*   **state (VARCHAR(255), Null)**: State/Province.
*   **postal_code (VARCHAR(20), Null)**: Postal/Zip code.
*   **start_date (DATE, Null)**: Staff member's start date.
*   **end_date (DATE, Null)**: Staff member's end date (if applicable, for tracking staff history).
*   **is_active (BOOLEAN, Default: TRUE)**: Indicates if the staff member is currently active.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the staff record was created.
*   **updated_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the staff record was last updated.

#### 4. Products Dimension (`DimProducts`)

*   **product_id (PK, UUID, Auto-generated)**: Unique identifier for each product (medication, supply, vaccine, etc.).
*   **product_category_id (FK, UUID, Null, References: `LookupProductCategories`)**: Foreign key to the `LookupProductCategories` table (see Lookup Tables below).
*   **product_name (VARCHAR(255), Not Null)**: Name of the product.
*   **description (TEXT, Null)**: Detailed product description.
*   **unit_cost (DECIMAL(10, 2), Null)**: Cost per unit (for COGS calculation).
*   **unit_price (DECIMAL(10, 2), Not Null)**: Selling price per unit.
*   **unit_of_measure (VARCHAR(255), Null)**: Unit of measure (e.g., each, ml, tablet). Consider a lookup table if standardized.
*   **is_active (BOOLEAN, Default: TRUE)**: Indicates if the product is currently active and available for sale/use.
*   **is_inventory_item (BOOLEAN, Default: TRUE)**: Flag to indicate if this product is tracked in inventory.
*   **reorder_level (INTEGER, Default: 0)**: Reorder level for inventory alerts.
*   **reorder_quantity (INTEGER, Default: 0)**: Quantity to reorder when stock reaches reorder level.
*   **supplier_id (FK, UUID, Null, References: `DimSuppliers`)**: Foreign key to the `DimSuppliers` table.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the product record was created.
*   **updated_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the product record was last updated.

#### 5. Services Dimension (`DimServices`)

*   **service_id (PK, UUID, Auto-generated)**: Unique identifier for each service offered (consultation, surgery, vaccination administration, etc.).
*   **service_category_id (FK, UUID, Null, References: `LookupServiceCategories`)**: Foreign key to the `LookupServiceCategories` table (see Lookup Tables below).
*   **service_name (VARCHAR(255), Not Null)**: Name of the service.
*   **description (TEXT, Null)**: Service description.
*   **service_price (DECIMAL(10, 2), Not Null)**: Price of the service.
*   **duration_minutes (INTEGER, Null)**: Expected duration of the service in minutes for appointment scheduling.
*   **is_active (BOOLEAN, Default: TRUE)**: Indicates if the service is currently offered.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the service record was created.
*   **updated_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the service record was last updated.

#### 6. Suppliers Dimension (`DimSuppliers`)

*   **supplier_id (PK, UUID, Auto-generated)**: Unique identifier for each supplier.
*   **supplier_name (VARCHAR(255), Not Null)**: Name of the supplier.
*   **contact_name (VARCHAR(255), Null)**: Contact person at the supplier.
*   **contact_phone (VARCHAR(20), Null)**: Supplier's phone number.
*   **email (VARCHAR(255), Null)**: Supplier's email.
*   **address_line1 (VARCHAR(255), Null)**: Address line 1.
*   **address_line2 (VARCHAR(255), Null)**: Address line 2 (optional).
*   **city (VARCHAR(255), Null)**: City.
*   **state (VARCHAR(255), Null)**: State/Province.
*   **postal_code (VARCHAR(20), Null)**: Postal/Zip code.
*   **notes (TEXT, Null)**: Notes about the supplier.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the supplier record was created.
*   **updated_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the supplier record was last updated.

#### 7. Expense Categories Dimension (`DimExpenseCategories`)

*   **expense_category_id (PK, UUID, Auto-generated)**: Unique identifier for each expense category (e.g., Rent, Utilities, Supplies).
*   **category_name (VARCHAR(255), Not Null)**: Name of the expense category.
*   **description (TEXT, Null)**: Category description (optional).
*   **is_active (BOOLEAN, Default: TRUE)**: Indicates if the category is currently active.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the category record was created.
*   **updated_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the category record was last updated.

#### 8. Payment Methods Dimension (`DimPaymentMethods`)

*   **payment_method_id (PK, UUID, Auto-generated)**: Unique identifier for each payment method (Cash, Card, Check, etc.).
*   **method_name (VARCHAR(255), Not Null)**: Name of the payment method.
*   **description (TEXT, Null)**: Description of the payment method (optional).
*   **is_active (BOOLEAN, Default: TRUE)**: Indicates if the payment method is currently active.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the payment method record was created.
*   **updated_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the payment method record was last updated.

#### 9. Locations Dimension (`DimLocations`)

*   **location_id (PK, UUID, Auto-generated)**: Unique identifier for each location (Clinic, Warehouse, Storage Room, etc.). Useful if a clinic has multiple locations.
*   **location_name (VARCHAR(255), Not Null)**: Name of the location.
*   **location_type (VARCHAR(255), Null)**: Type of location (Clinic, Warehouse, Storage Room, etc.). Consider a lookup table for types.
*   **address_line1 (VARCHAR(255), Null)**: Address line 1.
*   **address_line2 (VARCHAR(255), Null)**: Address line 2 (optional).
*   **city (VARCHAR(255), Null)**: City.
*   **state (VARCHAR(255), Null)**: State/Province.
*   **postal_code (VARCHAR(20), Null)**: Postal/Zip code.
*   **contact_phone (VARCHAR(20), Null)**: Location's phone number.
*   **notes (TEXT, Null)**: Notes about the location.
*   **is_active (BOOLEAN, Default: TRUE)**: Indicates if the location is currently active.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the location record was created.
*   **updated_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the location record was last updated.
