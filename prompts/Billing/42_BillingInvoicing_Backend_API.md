# 42_BillingInvoicing_Backend_API.md

**Core Feature:** Billing and Invoicing - Backend API Endpoints (NestJS)

**Purpose:** Define the RESTful API endpoints for managing Invoices and Payments in the backend NestJS application.

**Instructions for AI Agent:**

*   **Generate NestJS controller code** with the API endpoints defined below.
*   Use `/invoices` and `/payments` as base paths for controllers.
*   For each endpoint, generate:
    *   NestJS Controller decorator (`@Controller`)
    *   Route path decorators (`@Get`, `@Post`, `@Put`, `@Delete`)
    *   Method signature with parameters and types (TypeScript).
    *   Basic method body calling service methods (placeholders to services in `43_BillingInvoicing_Backend_Services.md`).
    *   Appropriate HTTP status codes.
    *   Authentication (`@UseGuards(JwtAuthGuard)`).
    *   Basic validation using NestJS `ValidationPipe` and DTOs.

---

## API Endpoints:

### Invoice Endpoints (`/invoices`):

*   **POST /invoices**: Create a new Invoice.
    *   Method: `POST`
    *   Request Body: `CreateInvoiceDto` (Define DTO with fields from `Invoices` table - client_id, invoice_date, due_date, invoice_status, notes, and potentially line items array - validation required). *Consider how to handle invoice line items creation - nested DTOs or separate endpoint? For now, assume nested DTOs in `CreateInvoiceDto`*.
    *   Response: `InvoiceDto`, 201 Created.

*   **GET /invoices**: Get a list of Invoices (with pagination, filtering, search, date range, status).
    *   Method: `GET`
    *   Query Parameters:
        *   Pagination: `page`, `pageSize`
        *   Filtering: `clientId`, `invoiceStatus`, `invoiceDate` range, `dueDate` range
        *   Search: (Consider search fields - client name, invoice number?)
        *   Sorting: (e.g., by invoice date, invoice number, balance due)
    *   Response: `PaginatedResponse<InvoiceDto[]>`, 200 OK.

*   **GET /invoices/:invoiceId**: Get a specific Invoice by ID, including line items and payment history.
    *   Method: `GET`
    *   Path Parameter: `invoiceId` (UUID)
    *   Response: `InvoiceDto` (including nested `InvoiceLineDto[]` and `PaymentDto[]` or summaries), 200 OK.

*   **PUT /invoices/:invoiceId**: Update an existing Invoice (header information - not line items directly through this endpoint, line items managed separately).
    *   Method: `PUT`
    *   Path Parameter: `invoiceId` (UUID)
    *   Request Body: `UpdateInvoiceDto` (Partial updates, validation, exclude line items for now).
    *   Response: `InvoiceDto`, 200 OK.

*   **DELETE /invoices/:invoiceId**: Delete an Invoice.
    *   Method: `DELETE`
    *   Path Parameter: `invoiceId` (UUID)
    *   Response: 204 No Content.

### Invoice Line Item Endpoints (`/invoices/:invoiceId/lines`):

*   **POST /invoices/:invoiceId/lines**: Add a new line item to an Invoice.
    *   Method: `POST`
    *   Path Parameter: `invoiceId` (UUID)
    *   Request Body: `CreateInvoiceLineDto` (Define DTO with fields from `FactInvoiceLines` - product_id, service_id, description, quantity, unit_price - validation).
    *   Response: `InvoiceLineDto`, 201 Created.

*   **GET /invoices/:invoiceId/lines**: Get all line items for a specific Invoice.
    *   Method: `GET`
    *   Path Parameter: `invoiceId` (UUID)
    *   Response: `InvoiceLineDto[]`, 200 OK.

*   **GET /invoices/:invoiceId/lines/:lineItemId**: Get a specific line item by ID for an Invoice.
    *   Method: `GET`
    *   Path Parameters: `invoiceId` (UUID), `lineItemId` (UUID)
    *   Response: `InvoiceLineDto`, 200 OK.

*   **PUT /invoices/:invoiceId/lines/:lineItemId**: Update an existing line item for an Invoice.
    *   Method: `PUT`
    *   Path Parameters: `invoiceId` (UUID), `lineItemId` (UUID)
    *   Request Body: `UpdateInvoiceLineDto` (Partial updates, validation).
    *   Response: `InvoiceLineDto`, 200 OK.

*   **DELETE /invoices/:invoiceId/lines/:lineItemId**: Delete a line item from an Invoice.
    *   Method: `DELETE`
    *   Path Parameters: `invoiceId` (UUID), `lineItemId` (UUID)
    *   Response: 204 No Content.


### Payment Endpoints (`/payments`):

*   **POST /payments**: Record a new Payment.
    *   Method: `POST`
    *   Request Body: `CreatePaymentDto` (Define DTO with fields from `FactPayments` - invoice_id, payment_method_id, payment_date, payment_amount, transaction_reference, notes - validation).
    *   Response: `PaymentDto`, 201 Created.

*   **GET /payments**: Get a list of Payments (with pagination, filtering, search, date range).
    *   Method: `GET`
    *   Query Parameters:
        *   Pagination: `page`, `pageSize`
        *   Filtering: `invoiceId`, `paymentMethodId`, `paymentDate` range
        *   Search: *(Consider search fields - client name, invoice number, transaction reference?)*
        *   Sorting: (e.g., by payment date, payment amount)
    *   Response: `PaginatedResponse<PaymentDto[]>`, 200 OK.

*   **GET /payments/:paymentId**: Get a specific Payment by ID.
    *   Method: `GET`
    *   Path Parameter: `paymentId` (UUID)
    *   Response: `PaymentDto`, 200 OK.

*   **PUT /payments/:paymentId**: Update an existing Payment (mainly notes or reference).
    *   Method: `PUT`
    *   Path Parameter: `paymentId` (UUID)
    *   Request Body: `UpdatePaymentDto` (Partial updates, validation).
    *   Response: `PaymentDto`, 200 OK.

*   **DELETE /payments/:paymentId**: Delete a Payment.
    *   Method: `DELETE`
    *   Path Parameter: `paymentId` (UUID)
    *   Response: 204 No Content.


### Invoice Reporting Endpoints (`/reports/invoices`):

*   **GET /reports/invoices/summary**: Get a summary of invoices for a date range.
    *   Method: `GET`
    *   Query Parameters:
        *   `startDate` (Date, required)
        *   `endDate` (Date, required)
    *   Response: `InvoiceSummaryDto` (Define DTO with summary fields - total invoices, total amount invoiced, total amount paid, total balance due), 200 OK.
    *   Description: Provides a summary of invoice financial data within a specified period.

*   **GET /reports/invoices/outstanding**: Get a list of outstanding (unpaid or partially paid) invoices.
    *   Method: `GET`
    *   Query Parameters:
        *   Pagination: `page`, `pageSize`
        *   Sorting: (e.g., by balance due, due date)
    *   Response: `PaginatedResponse<InvoiceDto[]>`, 200 OK.
    *   Description: Retrieves a list of invoices with a balance due.

---