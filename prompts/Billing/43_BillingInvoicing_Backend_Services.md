# 43_BillingInvoicing_Backend_Services.md

**Core Feature:** Billing and Invoicing - Backend Services (NestJS)

**Purpose:** Define the NestJS services for managing Invoices and Payments, and generating basic invoice reports, interacting with Prisma ORM.

**Instructions for AI Agent:**

*   **Generate NestJS service classes:** `InvoiceService`, `PaymentService`.
*   For each service method (corresponding to API endpoints in `42_BillingInvoicing_Backend_API.md`):
    *   Method signature with parameters and return types (TypeScript).
    *   Prisma Client interactions for CRUD operations, pagination, filtering, sorting, and searching.
    *   Implement logic for managing invoice line items and payments (potentially using nested Prisma operations or transactions if needed for data consistency).
    *   Implement logic for calculating invoice totals and balance due (consider generated columns in database, but calculations might also be needed in services).
    *   Error handling (Prisma exceptions, HTTP exceptions).
    *   Logging.
    *   Mapping Prisma entities to DTOs.
    *   Implement report generation logic for invoice summaries and outstanding invoices using Prisma aggregations and queries.

---

## Service Definitions:

### InvoiceService:

*   **createInvoice(createInvoiceDto: CreateInvoiceDto): Promise<InvoiceDto>**: Prisma `create` for `Invoices` table, potentially with nested `createMany` for `FactInvoiceLines` if handling line items creation within this method (or separate service methods for line items).  Calculate `total_amount` based on line items before creating invoice.
*   **getInvoices(queryParams: GetInvoicesQueryParams): Promise<PaginatedResponse<InvoiceDto[]>>**: Prisma `findMany`, `count` with pagination, filtering (on `clientId`, `invoiceStatus`, `invoiceDate` range, `dueDate` range, optional search), sorting.  Include related `FactInvoiceLines` and `FactPayments` data (or summaries) if efficient, or fetch them separately if needed for performance.
*   **getInvoiceById(invoiceId: UUID): Promise<InvoiceDto>**: Prisma `findUnique` by `invoiceId`, include related `FactInvoiceLines` and `FactPayments`. `NotFoundException` if not found.
*   **updateInvoice(invoiceId: UUID, updateInvoiceDto: UpdateInvoiceDto): Promise<InvoiceDto>**: Prisma `update` for `Invoices` table. `NotFoundException` if not found.  *(For now, exclude line item updates via this method, manage lines separately)*
*   **deleteInvoice(invoiceId: UUID): Promise<void>**: Prisma `delete` for `Invoices` table.  Consider cascade delete implications for `FactInvoiceLines` and `FactPayments`. `NotFoundException` if not found.

*   **createInvoiceLine(invoiceId: UUID, createInvoiceLineDto: CreateInvoiceLineDto): Promise<InvoiceLineDto>**: Prisma `create` for `FactInvoiceLines` linked to `invoiceId`.  Recalculate `total_amount` on `Invoices` table after adding a line item.
*   **getInvoiceLines(invoiceId: UUID): Promise<InvoiceLineDto[]>**: Prisma `findMany` for `FactInvoiceLines` filtered by `invoiceId`.
*   **getInvoiceLineById(invoiceId: UUID, lineItemId: UUID): Promise<InvoiceLineDto>**: Prisma `findUnique` for `FactInvoiceLines` by `lineItemId` and `invoiceId`.  `NotFoundException` if not found.
*   **updateInvoiceLine(invoiceId: UUID, lineItemId: UUID, updateInvoiceLineDto: UpdateInvoiceLineDto): Promise<InvoiceLineDto>**: Prisma `update` for `FactInvoiceLines` by `lineItemId` and `invoiceId`. Recalculate `total_amount` on `Invoices` table after updating a line item. `NotFoundException` if not found.
*   **deleteInvoiceLine(invoiceId: UUID, lineItemId: UUID): Promise<void>**: Prisma `delete` for `FactInvoiceLines` by `lineItemId` and `invoiceId`. Recalculate `total_amount` on `Invoices` table after deleting a line item.  `NotFoundException` if not found.

### PaymentService:

*   **createPayment(createPaymentDto: CreatePaymentDto): Promise<PaymentDto>**: Prisma `create` for `FactPayments`. Update `amount_paid` and `balance_due` on the related `Invoices` table after recording payment. Update `invoice_status` based on `balance_due`.
*   **getPayments(queryParams: GetPaymentsQueryParams): Promise<PaginatedResponse<PaymentDto[]>>**: Prisma `findMany`, `count` with pagination, filtering (on `invoiceId`, `paymentMethodId`, `paymentDate` range, optional search), sorting.
*   **getPaymentById(paymentId: UUID): Promise<PaymentDto>**: Prisma `findUnique` by `paymentId`. `NotFoundException` if not found.
*   **updatePayment(paymentId: UUID, updatePaymentDto: UpdatePaymentDto): Promise<PaymentDto>**: Prisma `update` for `FactPayments`. `NotFoundException` if not found. *(Consider what fields are actually updatable for payments - mainly notes/reference?)*
*   **deletePayment(paymentId: UUID): Promise<void>**: Prisma `delete` for `FactPayments`. Update `amount_paid` and `balance_due` on related `Invoices` table after deleting payment. Revert `invoice_status` if needed.  `NotFoundException` if not found.


### InvoiceReportService (or within InvoiceService if preferred):

*   **getInvoiceSummary(startDate: Date, endDate: Date): Promise<InvoiceSummaryDto>**:  Prisma aggregate queries to calculate:
    *   Total number of invoices created within the date range.
    *   Sum of `total_amount` for invoices within the date range.
    *   Sum of `amount_paid` for payments received within the date range (or linked to invoices in the date range - clarify requirement).
    *   Calculate total `balance_due` for invoices within the date range.
    *   Return `InvoiceSummaryDto` with these aggregated values.

*   **getOutstandingInvoices(queryParams: PaginationQueryParams): Promise<PaginatedResponse<InvoiceDto[]>>**: Prisma `findMany`, `count` for `Invoices` where `balance_due > 0`. Implement pagination and optional sorting (by `balance_due`, `due_date`). Return `PaginatedResponse<InvoiceDto[]>`.

---