# 62_ReportingAnalytics_Backend_API.md

**Core Feature:** Reporting and Analytics - Backend API Endpoints (NestJS)

**Purpose:** Define the RESTful API endpoints for retrieving various reports and analytics data in the backend NestJS application.

**Instructions for AI Agent:**

*   **Generate NestJS controller code** with the API endpoints defined below.
*   Use `/reports` as the base path for the controller.
*   For each endpoint, generate:
    *   NestJS Controller decorator (`@Controller`) - with `/reports` base path.
    *   Route path decorators (`@Get`) - Reports are primarily read-only data retrieval.
    *   Method signature with parameters and types (TypeScript).
    *   Basic method body calling service methods (placeholders to services in `63_ReportingAnalytics_Backend_Services.md`).
    *   Appropriate HTTP status codes (200 OK for successful report retrieval).
    *   Authentication (`@UseGuards(JwtAuthGuard)`).
    *   Define DTOs for report responses to structure the data returned by each endpoint.

---

## API Endpoints (`/reports`):

### Dashboard Data Endpoints (`/reports/dashboard`):

*   **GET /reports/dashboard/summary**: Get a summary dashboard data (key metrics).
    *   Method: `GET`
    *   Query Parameters:
        *   `startDate` (Date, optional, default to current month start)
        *   `endDate` (Date, optional, default to current date)
    *   Response: `DashboardSummaryDto` (Define DTO with fields like `totalRevenue`, `totalAppointments`, `totalPatientVisits`, `newClients`, `newPatients` for the specified period), 200 OK.
    *   Description: Returns summarized key metrics for the clinic.

### Financial Report Endpoints (`/reports/financial`):

*   **GET /reports/financial/revenue**: Get a revenue report.
    *   Method: `GET`
    *   Query Parameters:
        *   `startDate` (Date, required)
        *   `endDate` (Date, required)
        *   `groupBy` (ENUM: 'day', 'week', 'month', optional, default 'month') - for time series revenue trend.
        *   `category` (ENUM: 'service', 'product', 'all', optional, default 'all') - to filter by revenue category.
        *   `staffId` (UUID, optional) - filter by staff member.
        *   `locationId` (UUID, optional) - filter by location.
    *   Response: `RevenueReportDto[]` (Define DTO with fields like `dateGroup`, `revenueAmount`, `category`, `staffName`, `locationName` based on query parameters), 200 OK.
    *   Description: Returns revenue data, grouped and filtered as requested.

*   **GET /reports/financial/expenses**: Get an expense report.
    *   Method: `GET`
    *   Query Parameters:  *(Similar to revenue report - `startDate`, `endDate`, `groupBy`, `categoryId`, `locationId`)*
    *   Response: `ExpenseReportDto[]`, 200 OK.

*   **GET /reports/financial/profit-loss**: Get a profit and loss statement.
    *   Method: `GET`
    *   Query Parameters:  *(Similar to revenue/expense reports - `startDate`, `endDate`, `groupBy`)*
    *   Response: `ProfitLossStatementDto[]` (or a single summary DTO if not grouped over time), 200 OK.

*   **GET /reports/financial/payments**: Get a payment summary report.
    *   Method: `GET`
    *   Query Parameters:  *(Similar date range filters)*
    *   Response: `PaymentSummaryReportDto[]`, 200 OK.


### Operational Report Endpoints (`/reports/operational`):

*   **GET /reports/operational/appointments**: Get an appointment report.
    *   Method: `GET`
    *   Query Parameters:
        *   `startDate` (Date, required)
        *   `endDate` (Date, required)
        *   `groupBy` (ENUM: 'day', 'week', 'month', optional, default 'month')
        *   `appointmentType` (UUID, optional) - filter by appointment type
        *   `staffId` (UUID, optional) - filter by staff
        *   `status` (ENUM of Appointment Statuses, optional) - filter by appointment status
    *   Response: `AppointmentReportDto[]`, 200 OK.

*   **GET /reports/operational/patient-visits**: Get a patient visit report.
    *   Method: `GET`
    *   Query Parameters: *(Similar to appointment report - `startDate`, `endDate`, `groupBy`, `staffId`, `locationId`)*
    *   Response: `PatientVisitReportDto[]`, 200 OK.

*   **GET /reports/operational/staff-performance**: Get staff performance report.
    *   Method: `GET`
    *   Query Parameters:
        *   `startDate` (Date, required)
        *   `endDate` (Date, required)
        *   `groupBy` (ENUM: 'staff', optional, default 'staff' - or consider 'month', 'week' for staff performance over time)
    *   Response: `StaffPerformanceReportDto[]`, 200 OK.


### Patient/Client Report Endpoints (`/reports/demographics`):

*   **GET /reports/demographics/patients**: Get patient demographic report.
    *   Method: `GET`
    *   Query Parameters:  *(Consider filters - animal type, breed?)*
    *   Response: `PatientDemographicsReportDto` (or `PatientDemographicsBreakdownDto[]` for breakdowns by type/breed/age etc.), 200 OK.

*   **GET /reports/demographics/clients**: Get client demographic report.
    *   Method: `GET`
    *   Query Parameters: *(Consider filters - location?)*
    *   Response: `ClientDemographicsReportDto` (or `ClientDemographicsBreakdownDto[]` for location breakdowns), 200 OK.


---