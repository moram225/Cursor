# 20_PatientManagement_Overview.md

**Core Feature:** Patient and Client Management

**Purpose:** This feature provides the foundation for managing all client and patient information within the Veterinary PMS. It allows clinic staff to efficiently record, access, and update client details (pet owners) and patient details (animals), including medical history, appointments, and communication preferences.

**User Stories:**

*   As a **Receptionist**, I want to easily (in less than 2 clicks) **create new client records** when a new pet owner contacts the clinic.
*   As a **Receptionist**, I want to **search for existing clients** quickly using name, phone number, or email to schedule appointments or update information.
*   As a **Receptionist**, I want to **view client details** including contact information, address, and notes.

*   As a **Veterinarian**, I want to **create detailed patient records** including species, breed, date of birth, and medical history.
*   As a **Veterinarian**, I want to **access a patient's complete medical history** quickly during a consultation, including past visits, vaccinations, medications, and lab results.
*   As a **Veterinarian/Technician**, I want to **update patient records** with new medical findings, treatments, and observations after each visit.
*   As a **Clinic Administrator**, I want to be able to **generate reports** on client demographics and patient types to understand our client base.
*   As a **Client**, I have a **preference for how I want to be contacted** (email, SMS, etc.) and I want to be able to update this information easily.
*   As a **Client**, I might have multiple pets.
*   As a **Client**, I want to be able to **update my contact information** and communication preferences through a client portal (future enhancement - initially internal staff management).
*   As a **Client**, I want to be able to **view my pet's medical history** and vaccination records.
*   As a **Client**, I want to be able to **schedule appointments** and **view my pet's medical records** online.


**Key Functionalities:**

*   **Client Management:**
    *   Create, Read, Update, and Delete (CRUD) client records.
    *   Search and filter clients by various criteria (name, phone, email, etc.).
    *   View detailed client information including:
        *   Contact details (phone, email, address)
        *   Notes
        *   History of patients owned
        *   Billing history (initially linked, detailed billing in Billing Feature)
    *   Potentially link to communication preferences (SMS/Email - detailed in Communication Tools Feature).

*   **Patient Management:**
    *   Create, Read, Update, and Delete (CRUD) patient records.
    *   Search and filter patients by various criteria (name, client, species, breed, microchip, etc.).
    *   View detailed patient information including:
        *   Basic demographics (name, species, breed, DOB, gender, color)
        *   Client owner information
        *   Medical history summary (detailed visit records in Patient Visits Feature)
        *   Vaccination and medication notes (summaries, detailed records in Patient Visits Feature)
        *   Microchip number
    *   Record patient transfers between clinic locations (detailed in Patient Transfers Feature).

**Database Tables (Relevant Entities):**

*   **DimClients**:  Stores client information.
*   **DimPatients**: Stores patient information, linked to `DimClients`.
*   *(Potentially related: Appointments, Reminders - though these are separate features, Patient/Client Management will interface with them)*

**Instructions for AI Agent:**

*   Based on this overview and the database schema defined in `10b_Database_Tenant_Dimension_Schema.md` and `10d_Database_Tenant_Transactional_Schema.md`,  proceed to create the Backend API definitions in `22_PatientManagement_Backend_API.md`.
*   Focus on creating API endpoints for basic CRUD operations for `Client` and `Patient` entities, as well as search/filtering capabilities.
*   Ensure API design follows RESTful principles and uses appropriate HTTP methods and status codes.
*   Consider security and access control (RBAC) when designing API endpoints (though detailed RBAC implementation will be defined later). Assume basic authentication is required for now.
*   After defining the API, generate the corresponding NestJS backend services in `23_PatientManagement_Backend_Services.md`.