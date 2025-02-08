# 10d_Database_Tenant_Transactional_Schema.md

**Purpose:** Defines the schema for Transactional/Supporting Tables within the Tenant Database (per clinic). These tables support core functionality and relationships and are normalized to 3NF.

**Instructions for AI Agent:**

*   **Generate Prisma schema definitions** for the Transactional/Supporting Tables described below.
*   Ensure accurate data types, constraints, and comments based on the descriptions.
*   Pay close attention to foreign key relationships linking to Dimension Tables and Fact Tables.
*   Note that these tables belong to the *Tenant Database* and support core transactional operations.

---

## II. Tenant Database (Per Clinic - Star Schema with 3NF) - C. Transactional/Supporting Tables

These tables support the core functionality and relationships but are not strictly dimensions or facts.

#### 1. Appointments Table (`Appointments`)

*   **appointment_id (PK, UUID, Auto-generated)**: Unique identifier for each appointment.
*   **patient_id (FK, UUID, Not Null, References: `DimPatients`)**: Foreign key to `DimPatients`.
*   **client_id (FK, UUID, Not Null, References: `DimClients`)**: Foreign key to `DimClients`.
*   **staff_id (FK, UUID, Null, References: `DimStaff`)**: Preferred Staff member for the appointment (optional).
*   **appointment_type_id (FK, UUID, Null, References: `LookupAppointmentTypes`)**: Foreign key to `LookupAppointmentTypes` (see Lookup Tables below).
*   **appointment_date (DATE, Not Null)**: Date of the appointment.
*   **start_time (TIME WITH TIME ZONE, Not Null)**: Start time of the appointment.
*   **end_time (TIME WITH TIME ZONE, Not Null)**: End time of the appointment.
*   **status (ENUM('Scheduled', 'Confirmed', 'Checked In', 'Completed', 'Cancelled', 'No Show'), Default: 'Scheduled')**: Status of the appointment.
*   **notes (TEXT, Null)**: Notes about the appointment.
*   **reminder_sent (BOOLEAN, Default: FALSE)**: Flag to indicate if reminder has been sent.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the appointment was created.
*   **updated_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the appointment was last updated.

#### 2. Invoices Table (`Invoices`)

*   **invoice_id (PK, UUID, Auto-generated)**: Unique identifier for each invoice.
*   **client_id (FK, UUID, Not Null, References: `DimClients`)**: Foreign key to `DimClients`.
*   **invoice_number (VARCHAR(255), Unique, Not Null)**: Invoice number (consider a clinic-specific prefix/sequence).
*   **invoice_date (DATE, Not Null)**: Date of the invoice.
*   **due_date (DATE, Null)**: Due date for payment.
*   **invoice_status (ENUM('Draft', 'Sent', 'Paid', 'Partially Paid', 'Overdue', 'Void'), Default: 'Draft')**: Status of the invoice.
*   **total_amount (DECIMAL(10, 2), Not Null, Default: 0)**: Total amount of the invoice (sum of line totals - calculated in application or triggers).
*   **amount_paid (DECIMAL(10, 2), Not Null, Default: 0)**: Total amount paid against the invoice (calculated from `FactPayments`).
*   **balance_due (DECIMAL(10, 2), Not Null, Generated Column as `total_amount - amount_paid`)**: Balance due on the invoice.
*   **notes (TEXT, Null)**: Invoice notes/terms.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the invoice was created.
*   **updated_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the invoice was last updated.

#### 3. Orders Table (`Orders`)

*   **order_id (PK, UUID, Auto-generated)**: Unique identifier for each purchase order.
*   **supplier_id (FK, UUID, Not Null, References: `DimSuppliers`)**: Foreign key to `DimSuppliers`.
*   **order_number (VARCHAR(255), Unique, Not Null)**: Purchase order number (consider clinic-specific prefix/sequence).
*   **order_date (DATE, Not Null)**: Date of the purchase order.
*   **order_status (ENUM('Draft', 'Placed', 'Partially Received', 'Received', 'Cancelled'), Default: 'Draft')**: Status of the purchase order.
*   **total_amount (DECIMAL(10, 2), Not Null, Default: 0)**: Total amount of the order (sum of line totals - calculated in application or triggers).
*   **notes (TEXT, Null)**: Order notes/terms.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the order was created.
*   **updated_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the order was last updated.

#### 4. Lab Results Table (`LabResults`)

*   **lab_result_id (PK, UUID, Auto-generated)**: Unique identifier for each lab result record.
*   **patient_visit_id (FK, UUID, Not Null, References: `FactPatientVisits`)**: Foreign key to `FactPatientVisits`.
*   **lab_test_type (VARCHAR(255), Not Null)**: Type of lab test performed (e.g., "CBC", "Blood Chemistry"). Consider a lookup table.
*   **result_date (DATE, Not Null)**: Date the lab results were received.
*   **result_value (TEXT, Null)**: The lab result value (could be numeric, text, or link to a file).
*   **units (VARCHAR(255), Null)**: Units for the result value (if applicable).
*   **reference_range (VARCHAR(255), Null)**: Normal reference range for the test.
*   **abnormal_flags (VARCHAR(255), Null)**: Flags indicating abnormal results (e.g., "H" for high, "L" for low).
*   **lab_notes (TEXT, Null)**: Notes from the lab or vet about the results.
*   **lab_system_reference (VARCHAR(255), Null)**: Reference ID in the external lab system if integrated.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the lab result was recorded.
*   **updated_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the lab result was last updated.

#### 5. Reminders Table (`Reminders`)

*   **reminder_id (PK, UUID, Auto-generated)**: Unique identifier for each reminder.
*   **patient_id (FK, UUID, Not Null, References: `DimPatients`)**: Foreign key to `DimPatients`.
*   **client_id (FK, UUID, Not Null, References: `DimClients`)**: Foreign key to `DimClients`.
*   **reminder_type (VARCHAR(255), Not Null)**: Type of reminder (e.g., "Vaccination Due", "Appointment Reminder", "Check-up"). Consider a lookup table.
*   **reminder_date (DATE, Not Null)**: Date for the reminder (e.g., date vaccination is due).
*   **send_date (TIMESTAMP WITH TIME ZONE, Null)**: Date and time the reminder was actually sent.
*   **communication_channel (ENUM('SMS', 'Email', 'App Notification', 'Printed'), Default: 'Email')**: Channel used for the reminder.
*   **status (ENUM('Pending', 'Sent', 'Failed', 'Acknowledged'), Default: 'Pending')**: Status of the reminder.
*   **notes (TEXT, Null)**: Notes about the reminder.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the reminder was created.
*   **updated_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the reminder was last updated.

#### 6. Patient Transfers Table (`PatientTransfers`)

*   **patient_transfer_id (PK, UUID, Auto-generated)**: Unique identifier for each patient transfer record.
*   **patient_id (FK, UUID, Not Null, References: `DimPatients`)**: Foreign key to `DimPatients`.
*   **from_location_id (FK, UUID, Not Null, References: `DimLocations`)**: Foreign key to `DimLocations` indicating the location the patient is transferred *from*.
*   **to_location_id (FK, UUID, Not Null, References: `DimLocations`)**: Foreign key to `DimLocations` indicating the location the patient is transferred *to*.
*   **transfer_date (DATE, Not Null)**: Date of the patient transfer.
*   **reason_for_transfer (TEXT, Null)**: Reason for the transfer.
*   **transfer_notes (TEXT, Null)**: Notes about the transfer.
*   **staff_id (FK, UUID, Null, References: `DimStaff`)**: Staff member initiating the transfer.
*   **created_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the transfer record was created.
*   **updated_at (TIMESTAMP WITH TIME ZONE, Default: NOW())**: Timestamp when the transfer record was last updated.