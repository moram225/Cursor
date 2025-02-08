# 30_AppointmentVisits_Overview.md

**Core Feature:** Appointment Scheduling and Patient Visits

**Purpose:** This feature enables clinic staff to efficiently schedule appointments and record patient visit details. It integrates with Patient and Client Management to link appointments and visits to specific patients and clients. This feature is crucial for managing clinic workflow, tracking patient encounters, and generating billing information.

**User Stories:**

*   As a **Receptionist**, I want to easily **schedule appointments** for clients and patients, specifying date, time, appointment type, and preferred staff member.
*   As a **Receptionist**, I want to **view the appointment schedule** for a day, week, or month to manage bookings and identify available slots.
*   As a **Receptionist**, I want to **view the appointment schedule** for a specific patient or client to manage bookings and identify available slots.
*   As a **Receptionist**, I want to **view the appointment schedule** for a specific staff member to manage bookings and identify available slots.
*   As a **Receptionist**, I want to **view the available slots of the veterinarian in one calendar view, on the y-axis of the calendar, the veterinarian's name, and on the x-axis the date and time of the appointment** to manage bookings and identify available slots.
*   As a **Receptionist**, I want to **reschedule or cancel appointments** as needed.
*   As a **Veterinarian**, I want to **see my daily schedule** of appointments.

*   As a **Veterinarian**, I want to **record details of a patient visit** after an appointment, including diagnosis, treatment notes, medications, vaccinations, and lab results.
*   As a **Veterinarian/Technician**, I want to **quickly access a patient's appointment history** and visit records.
*   As a **Clinic Administrator**, I want to **generate reports on appointment types and visit frequencies** to analyze clinic service utilization.

**Key Functionalities:**

*   **Appointment Scheduling:**
    *   Create, Read, Update, and Delete (CRUD) appointments.
    *   View appointment calendar/schedule (daily, weekly, monthly views).
    *   Filter appointments by staff, date, status, appointment type.
    *   Manage appointment statuses (Scheduled, Confirmed, Checked In, Completed, Cancelled, No Show).
    *   Assign staff members to appointments.
    *   Potentially integrate with reminder system (SMS/Email - detailed in Communication Tools Feature, but Appointment feature will trigger reminders).

*   **Patient Visit Recording:**
    *   Create, Read, Update, and Delete (CRUD) patient visit records.
    *   Link visit records to appointments (if applicable) and to patients and clients.
    *   Record visit date, time, reason for visit, diagnosis, treatment notes, vaccinations, medications, lab results summary, weight, temperature.
    *   Manage billing status of visits (Unbilled, Billed, Paid, Partially Paid - detailed billing in Billing Feature).
    *   View patient visit history.

**Database Tables (Relevant Entities):**

*   **Appointments**: Stores appointment scheduling information.
*   **FactPatientVisits**: Stores patient visit details.
*   **DimStaff**:  For assigning staff to appointments and visits.
*   **DimPatients**: To link appointments and visits to patients.
*   **DimClients**: To link appointments and visits to clients.
*   **DimLocations**: To record location of visit.
*   **LookupAppointmentTypes**: For categorizing appointment types.

**Instructions for AI Agent:**

*   Based on this overview and the database schema defined in `10d_Database_Tenant_Transactional_Schema.md`, `10c_Database_Tenant_Fact_Schema.md` and `10e_Database_Tenant_Lookup_Schema.md`,  proceed to create the Backend API definitions in `32_AppointmentVisits_Backend_API.md`.
*   Focus on creating API endpoints for CRUD operations for `Appointment` and `PatientVisit` entities, as well as schedule viewing, filtering, and reporting (basic reporting for now - listing and summaries).
*   Ensure API design follows RESTful principles and uses appropriate HTTP methods and status codes.
*   Consider security and access control (RBAC) when designing API endpoints. Assume basic authentication is required.
*   After defining the API, generate the corresponding NestJS backend services in `33_AppointmentVisits_Backend_Services.md`.