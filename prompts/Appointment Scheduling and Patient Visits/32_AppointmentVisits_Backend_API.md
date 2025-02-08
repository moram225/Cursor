# 32_AppointmentVisits_Backend_API.md

**Core Feature:** Appointment Scheduling and Patient Visits - Backend API Endpoints (NestJS)

**Purpose:** Define the RESTful API endpoints for managing Appointments and Patient Visits in the backend NestJS application.

**Instructions for AI Agent:**

*   **Generate NestJS controller code** with the API endpoints defined below.
*   For each endpoint, generate:
    *   NestJS Controller decorator (`@Controller`) - use `/appointments` and `/patient-visits` as base paths.
    *   Route path (`@Get`, `@Post`, `@Put`, `@Delete` decorators)
    *   Method signature with appropriate parameters and types (TypeScript).
    *   Basic method body calling service methods (placeholder calls to services defined in `33_AppointmentVisits_Backend_Services.md`).
    *   Appropriate HTTP status codes.
    *   Authentication (`@UseGuards(JwtAuthGuard)`).
    *   Basic validation using NestJS `ValidationPipe` and DTOs.

---

## API Endpoints:

### Appointment Endpoints (`/appointments`):

*   **POST /appointments**: Create a new Appointment.
    *   Method: `POST`
    *   Request Body: `CreateAppointmentDto` (Define DTO with fields from `Appointments` table - patient_id, client_id, staff_id, appointment_type_id, appointment_date, start_time, end_time, notes, status - with validation).
    *   Response: `AppointmentDto`, 201 Created.

*   **GET /appointments**: Get a list of Appointments (with pagination, filtering, search, date range).
    *   Method: `GET`
    *   Query Parameters:
        *   Pagination: `page`, `pageSize`
        *   Filtering: `staffId`, `patientId`, `clientId`, `appointmentTypeId`, `status`
        *   Date Range: `startDate`, `endDate` (for schedule view)
        *   Search:  *(Consider search fields - client name, patient name?)*
    *   Response: `PaginatedResponse<AppointmentDto[]>`, 200 OK.

*   **GET /appointments/:appointmentId**: Get a specific Appointment by ID.
    *   Method: `GET`
    *   Path Parameter: `appointmentId` (UUID)
    *   Response: `AppointmentDto`, 200 OK.

*   **PUT /appointments/:appointmentId**: Update an existing Appointment.
    *   Method: `PUT`
    *   Path Parameter: `appointmentId` (UUID)
    *   Request Body: `UpdateAppointmentDto` (Partial updates allowed, validation).
    *   Response: `AppointmentDto`, 200 OK.

*   **DELETE /appointments/:appointmentId**: Delete an Appointment.
    *   Method: `DELETE`
    *   Path Parameter: `appointmentId` (UUID)
    *   Response: 204 No Content.

### Patient Visit Endpoints (`/patient-visits`):

*   **POST /patient-visits**: Create a new Patient Visit record.
    *   Method: `POST`
    *   Request Body: `CreatePatientVisitDto` (Define DTO with fields from `FactPatientVisits` table - patient_id, client_id, staff_id, appointment_id, location_id, visit_date, visit_start_time, visit_end_time, reason_for_visit, diagnosis, treatment_notes, vaccinations_administered, medications_prescribed, lab_results_summary, weight, temperature, billing_status - with validation).
    *   Response: `PatientVisitDto`, 201 Created.

*   **GET /patient-visits**: Get a list of Patient Visits (with pagination, filtering, search).
    *   Method: `GET`
    *   Query Parameters:
        *   Pagination: `page`, `pageSize`
        *   Filtering: `staffId`, `patientId`, `clientId`, `locationId`, `visitDate` range, `billingStatus`
        *   Search: *(Consider search fields - patient name, client name, diagnosis?)*
    *   Response: `PaginatedResponse<PatientVisitDto[]>`, 200 OK.

*   **GET /patient-visits/:patientVisitId**: Get a specific Patient Visit by ID.
    *   Method: `GET`
    *   Path Parameter: `patientVisitId` (UUID)
    *   Response: `PatientVisitDto`, 200 OK.

*   **PUT /patient-visits/:patientVisitId**: Update an existing Patient Visit record.
    *   Method: `PUT`
    *   Path Parameter: `patientVisitId` (UUID)
    *   Request Body: `UpdatePatientVisitDto` (Partial updates, validation).
    *   Response: `PatientVisitDto`, 200 OK.

*   **DELETE /patient-visits/:patientVisitId**: Delete a Patient Visit record.
    *   Method: `DELETE`
    *   Path Parameter: `patientVisitId` (UUID)
    *   Response: 204 No Content.

---