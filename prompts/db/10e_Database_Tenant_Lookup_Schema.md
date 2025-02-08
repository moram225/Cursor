# 10e_Database_Tenant_Lookup_Schema.md

**Purpose:** Defines the schema for Lookup Tables within the Tenant Database (per clinic). These tables provide lists of valid values for data consistency and application options.

**Instructions for AI Agent:**

*   **Generate Prisma schema definitions** for the Lookup Tables described below.
*   Ensure accurate data types, constraints, and comments based on the descriptions.
*   Note that these tables belong to the *Tenant Database* and are used for data consistency and options management.

---

## II. Tenant Database (Per Clinic - Star Schema with 3NF) - D. Lookup Tables

These are simple tables to hold lists of valid values, improving data consistency and making it easier to manage options in the application.

These are simple tables to hold lists of valid values, improving data consistency and making it easier to manage options in the application.

#### 1. Lookup Product Categories (`LookupProductCategories`)
*   **product_category_id (PK, UUID, Auto-generated)**
*   **category_name (VARCHAR(255), Unique, Not Null)**
*   **description (TEXT, Null)**

#### 2. Lookup Service Categories (`LookupServiceCategories`)
*   **service_category_id (PK, UUID, Auto-generated)**
*   **category_name (VARCHAR(255), Unique, Not Null)**
*   **description (TEXT, Null)**

#### 3. Lookup Appointment Types (`LookupAppointmentTypes`)
*   **appointment_type_id (PK, UUID, Auto-generated)**
*   **type_name (VARCHAR(255), Unique, Not Null)**
*   **description (TEXT, Null)**

#### 4. Lookup Reminder Types (`LookupReminderTypes`)
*   **reminder_type_id (PK, UUID, Auto-generated)**
*   **type_name (VARCHAR(255), Unique, Not Null)**
*   **description (TEXT, Null)**

#### 5. Lookup Animal Types (Optional - `LookupAnimalTypes`)
*   **animal_type_id (PK, UUID, Auto-generated)**
*   **type_name (VARCHAR(255), Unique, Not Null)**
*   **description (TEXT, Null)**

#### 6. Lookup Breed Types (Optional - `LookupBreeds`)
*   **breed_id (PK, UUID, Auto-generated)**
*   **breed_name (VARCHAR(255), Unique, Not Null)**
*   **animal_type_id (FK, UUID, References: `LookupAnimalTypes`)**: // Link to animal type if breeds are animal-specific
*   **description (TEXT, Null)**

#### 7. Lookup Location Types (Optional - `LookupLocationTypes`)
*   **location_type_id (PK, UUID, Auto-generated)**
*   **type_name (VARCHAR(255), Unique, Not Null)**
*   **description (TEXT, Null)**

#### 8. Lookup Lab Test Types (Optional - `LookupLabTestTypes`)
*   **lab_test_type_id (PK, UUID, Auto-generated)**
*   **test_name (VARCHAR(255), Unique, Not Null)**
*   **description (TEXT, Null)**


---