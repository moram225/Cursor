# 63_ReportingAnalytics_Backend_Services.md

**Core Feature:** Reporting and Analytics - Backend Services (NestJS)

**Purpose:** Define the NestJS services for generating various reports and analytics data, interacting with Prisma ORM.

**Instructions for AI Agent:**

*   **Generate NestJS service class:** `ReportingService`.
*   For each service method (corresponding to API endpoints in `62_ReportingAnalytics_Backend_API.md`):
    *   Method signature with parameters and return types (TypeScript).
    *   Implement data aggregation logic using Prisma Client.  This will heavily rely on Prisma's aggregate functions (`groupBy`, `sum`, `count`, `avg`, etc.) and potentially raw SQL queries for complex aggregations or performance optimization if needed.
    *   For time-series reports (grouped by day, week, month), use Prisma's date functions or raw SQL date truncation/grouping functions.
    *   Error handling (Prisma exceptions, HTTP exceptions).
    *   Logging.
    *   Map aggregated data to appropriate Report DTOs.
    *   Focus on efficiency for report generation, especially for large datasets. Consider database indexing on relevant columns for report queries.

---

## Service Definitions:

### ReportingService:

#### Dashboard Data:

*   **getDashboardSummary(startDate: Date, endDate: Date): Promise<DashboardSummaryDto>**:
    *   Implementation:
        *   Use Prisma aggregate queries to:
            *   Calculate `totalRevenue` (sum of `total_amount` from `Invoices` within date range where `invoice_status` is 'Paid' or 'Partially Paid' - or based on payment dates - clarify requirement).
            *   Calculate `totalAppointments` (count of `Appointments` within date range).
            *   Calculate `totalPatientVisits` (count of `FactPatientVisits` within date range).
            *   Calculate `newClients` (count of `DimClients` created within date range).
            *   Calculate `newPatients` (count of `DimPatients` created within date range).
        *   Return `DashboardSummaryDto` with aggregated values.

#### Financial Reports:

*   **getRevenueReport(startDate: Date, endDate: Date, groupBy?: 'day' | 'week' | 'month', category?: 'service' | 'product' | 'all', staffId?: UUID, locationId?: UUID): Promise<RevenueReportDto[]>**:
    *   Implementation:
        *   Use Prisma `groupBy` and `aggregate` to calculate revenue.
        *   Group by the specified `groupBy` interval (day, week, month - use Prisma date functions or raw SQL for grouping).
        *   Filter invoices by `invoice_date` within the `startDate` and `endDate` range.
        *   Apply filters for `category` (service, product, or all), `staffId`, `locationId` using Prisma `where` clause.
        *   Join with `FactInvoiceLines`, `DimProducts`, `DimServices`, `DimStaff`, `DimLocations` as needed to get category names, staff names, location names for reporting.
        *   Return an array of `RevenueReportDto` objects.

*   **(Implement similar service methods for other financial reports: `getExpenseReport`, `getProfitLossStatement`, `getPaymentSummaryReport`):**
    *   Adapt Prisma queries and aggregations to use relevant fact tables (`FactExpenses`, `FactPayments`) and dimensions.
    *   Adjust grouping and filtering logic as needed for each report type.

#### Operational Reports:

*   **getAppointmentReport(startDate: Date, endDate: Date, groupBy?: 'day' | 'week' | 'month', appointmentType?: UUID, staffId?: UUID, status?: AppointmentStatus): Promise<AppointmentReportDto[]>**:
    *   Implementation:  Use Prisma to aggregate `Appointments` data, group by `groupBy`, filter by date range, `appointmentType`, `staffId`, `status`.

*   **(Implement similar service methods for `getPatientVisitReport`, `getStaffPerformanceReport`):**
    *   Adapt Prisma queries to aggregate `FactPatientVisits` and join with relevant dimensions for grouping and filtering.

#### Patient/Client Reports:

*   **getPatientDemographicsReport(filters?: PatientDemographicsFilters): Promise<PatientDemographicsReportDto | PatientDemographicsBreakdownDto[]>**:
    *   Implementation: Use Prisma aggregate queries on `DimPatients` to count patients by animal type, breed, age ranges, gender etc.  Handle optional filters.

*   **getClientDemographicsReport(filters?: ClientDemographicsFilters): Promise<ClientDemographicsReportDto | ClientDemographicsBreakdownDto[]>**:
    *   Implementation: Use Prisma aggregate queries on `DimClients` to count clients and potentially group by location (city, state etc.). Handle optional filters.

---