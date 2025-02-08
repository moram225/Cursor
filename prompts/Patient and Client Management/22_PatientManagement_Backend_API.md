# 22_PatientManagement_Backend_API.md

**Core Feature:** Patient and Client Management - Backend API Endpoints (NestJS)

**Purpose:** Define the RESTful API endpoints for managing Clients and Patients in the backend NestJS application. These endpoints will be used by the frontend to perform CRUD operations and data retrieval for Patient and Client Management.

**Instructions for AI Agent:**

*   **Generate NestJS controller code** with the API endpoints defined below.
*   For each endpoint, generate:
    *   NestJS Controller decorator (`@Controller`)
    *   Route path (`@Get`, `@Post`, `@Put`, `@Delete` decorators)
    *   Method signature with appropriate parameters and types (using TypeScript).
    *   Basic method body that calls the corresponding service method (which will be defined in `23_PatientManagement_Backend_Services.md`).  For now, just include a placeholder call like `return this.patientService.someMethod(...)`.
    *   Use appropriate HTTP status codes in responses (e.g., 200 OK, 201 Created, 400 Bad Request, 404 Not Found, 500 Internal Server Error).
    *   Apply basic authentication (e.g., `@UseGuards(JwtAuthGuard)`) to all relevant endpoints (excluding potentially public endpoints in the future, though for now assume all need auth).
    *   Consider basic validation using NestJS ValidationPipe (e.g., `@Body(ValidationPipe)` for POST/PUT requests). Define basic Data Transfer Objects (DTOs) for request bodies based on the entity structure, including validation decorators (e.g., `@IsString()`, `@IsNotEmpty()`, `@IsUUID()`, `@IsOptional()`).

---

## API Endpoints:

### Client Endpoints (`/clients`):

*   **POST /clients**: Create a new Client.
    *   Method: `POST`
    *   Request Body:  `CreateClientDto` (Define DTO with fields from `DimClients` table - client_name, contact_phone, email, address, notes - with validation rules).
    *   Response: `ClientDto` (on success - representation of the created client), 201 Created.  Error responses for validation errors (400), server errors (500).
    *   Description: Creates a new client record in the database.

*   **GET /clients**: Get a list of Clients (with optional pagination, filtering, and search).
    *   Method: `GET`
    *   Query Parameters (Optional):
        *   `page` (integer, default 1)
        *   `pageSize` (integer, default 10)
        *   `search` (string, for searching client_name, phone, email)
        *   `sortBy` (string, e.g., `client_name`, `created_at`, with optional `sortOrder` - 'asc' or 'desc')
    *   Response: `PaginatedResponse<ClientDto[]>` (on success - list of clients with pagination metadata), 200 OK. Error responses for server errors (500).
    *   Description: Retrieves a list of clients, supporting pagination, filtering by search term, and sorting.

*   **GET /clients/:clientId**: Get a specific Client by ID.
    *   Method: `GET`
    *   Path Parameter: `clientId` (UUID)
    *   Response: `ClientDto` (on success - details of the client), 200 OK. Error responses for Client not found (404), server errors (500).
    *   Description: Retrieves detailed information for a specific client based on their ID.

*   **PUT /clients/:clientId**: Update an existing Client.
    *   Method: `PUT`
    *   Path Parameter: `clientId` (UUID)
    *   Request Body: `UpdateClientDto` (Define DTO, similar to `CreateClientDto` but with all fields optional for partial updates, include validation).
    *   Response: `ClientDto` (on success - representation of the updated client), 200 OK. Error responses for Client not found (404), validation errors (400), server errors (500).
    *   Description: Updates the information for an existing client.

*   **DELETE /clients/:clientId**: Delete a Client.
    *   Method: `DELETE`
    *   Path Parameter: `clientId` (UUID)
    *   Response:  204 No Content (on successful deletion). Error responses for Client not found (404), server errors (500).
    *   Description: Deletes a client record from the database.  *Consider implications of deleting clients who have patients or invoices - discuss cascade delete or soft delete strategies later.*

### Patient Endpoints (`/patients`):

*   **(Define similar CRUD endpoints for patients - POST /patients, GET /patients, GET /patients/:patientId, PUT /patients/:patientId, DELETE /patients/:patientId)**

    *   Use `/patients` as the base path.
    *   Create DTOs: `CreatePatientDto`, `UpdatePatientDto`, `PatientDto` based on `DimPatients` table structure (patient_name, client_id, animal_type, breed, DOB, gender, color, medical_history, vaccination_notes, medication_notes, microchip_number).  Include validation rules in DTOs.
    *   Implement pagination, filtering, and search for `GET /patients` endpoint similar to clients.
    *   Ensure proper error handling and HTTP status codes.
    *   Apply authentication and basic validation.

---