# 33_AppointmentVisits_Backend_Services.md

**Core Feature:** Appointment Scheduling and Patient Visits - Backend Services (NestJS)

**Purpose:** Define the NestJS services for managing Appointments and Patient Visits, interacting with Prisma ORM.

**Instructions for AI Agent:**

*   **Generate NestJS service classes:** `AppointmentService`, `PatientVisitService`.
*   For each service method (corresponding to API endpoints in `32_AppointmentVisits_Backend_API.md`):
    *   Method signature with parameters and return types (TypeScript).
    *   Prisma Client interactions for CRUD operations, pagination, filtering, sorting, and searching.
    *   Error handling (Prisma exceptions, HTTP exceptions).
    *   Logging.
    *   Mapping Prisma entities to DTOs.

---

## Service Definitions:

### AppointmentService:

*   **createAppointment(createAppointmentDto: CreateAppointmentDto): Promise<AppointmentDto>**:  Prisma `create`.
*   **getAppointments(queryParams: GetAppointmentsQueryParams): Promise<PaginatedResponse<AppointmentDto[]>>**: Prisma `findMany`, `count` with pagination, filtering (on `staffId`, `patientId`, `clientId`, `appointmentTypeId`, `status`, date range - using Prisma's `where` clause and date functions), optional search (if defined in API).  Sorting if needed.
*   **getAppointmentById(appointmentId: UUID): Promise<AppointmentDto>**: Prisma `findUnique` by `appointmentId`.  `NotFoundException` if not found.
*   **updateAppointment(appointmentId: UUID, updateAppointmentDto: UpdateAppointmentDto): Promise<AppointmentDto>**: Prisma `update` by `appointmentId`.  `NotFoundException` if not found.
*   **deleteAppointment(appointmentId: UUID): Promise<void>**: Prisma `delete` by `appointmentId`. `NotFoundException` if not found.


### PatientVisitService:

*   **createPatientVisit(createPatientVisitDto: CreatePatientVisitDto): Promise<PatientVisitDto>**: Prisma `create`.
*   **getPatientVisits(queryParams: GetPatientVisitsQueryParams): Promise<PaginatedResponse<PatientVisitDto[]>>**: Prisma `findMany`, `count` with pagination, filtering (on `staffId`, `patientId`, `clientId`, `locationId`, `visitDate` range, `billingStatus`, optional search), sorting if needed.
*   **getPatientVisitById(patientVisitId: UUID): Promise<PatientVisitDto>**: Prisma `findUnique` by `patientVisitId`. `NotFoundException` if not found.
*   **updatePatientVisit(patientVisitId: UUID, updatePatientVisitDto: UpdatePatientVisitDto): Promise<PatientVisitDto>**: Prisma `update` by `patientVisitId`. `NotFoundException` if not found.
*   **deletePatientVisit(patientVisitId: UUID): Promise<void>**: Prisma `delete` by `patientVisitId`. `NotFoundException` if not found.


---