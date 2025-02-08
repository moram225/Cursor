# 23_PatientManagement_Backend_Services.md

**Core Feature:** Patient and Client Management - Backend Services (NestJS)

**Purpose:** Define the NestJS services that will handle the business logic for the Patient and Client Management API endpoints.  These services will interact with the Prisma ORM to access and manipulate data in the database.

**Instructions for AI Agent:**

*   **Generate NestJS service classes** (`ClientService`, `PatientService`) with methods corresponding to the API endpoints defined in `22_PatientManagement_Backend_API.md`.
*   For each service method, generate:
    *   Method signature with appropriate parameters and return types (using TypeScript).
    *   Implementation that uses Prisma Client to interact with the database.
    *   Error handling logic (e.g., using try-catch blocks to handle Prisma exceptions and throw appropriate HTTP exceptions - `NotFoundException`, `BadRequestException`, `InternalServerErrorException`).
    *   Basic logging (using NestJS built-in logger).
    *   For pagination, implement Prisma's `skip` and `take` options.
    *   For filtering and search, use Prisma's `where` clause.
    *   For sorting, use Prisma's `orderBy` clause.
    *   Map Prisma entities to DTOs (Data Transfer Objects) before returning them from the service methods to the controllers.

---

## Service Definitions:

### ClientService:

*   **createClient(createClientDto: CreateClientDto): Promise<ClientDto>**:
    *   Implementation: Use Prisma Client to create a new `Client` record in the database using data from `createClientDto`.
    *   Return:  The newly created `ClientDto`.

*   **getClients(queryParams: GetClientsQueryParams): Promise<PaginatedResponse<ClientDto[]>>**:
    *   QueryParams (interface `GetClientsQueryParams` - define properties for page, pageSize, search, sortBy, sortOrder):  To handle pagination, filtering, and sorting.
    *   Implementation:
        *   Use Prisma Client to fetch a list of `Client` records.
        *   Apply pagination using `skip` and `take` based on `page` and `pageSize` from queryParams.
        *   Apply filtering using Prisma's `where` clause based on `search` term (search in `client_name`, `contact_phone`, `email` fields - use `OR` condition).
        *   Apply sorting using Prisma's `orderBy` clause based on `sortBy` and `sortOrder` from queryParams.
        *   Fetch total count of clients (for pagination metadata) using Prisma's `count` aggregation.
    *   Return: `PaginatedResponse<ClientDto[]>` object containing the list of `ClientDto`s and pagination metadata (total items, current page, page size, etc.).  *(Define `PaginatedResponse` interface)*.

*   **getClientById(clientId: UUID): Promise<ClientDto>**:
    *   Implementation: Use Prisma Client to find a `Client` record by `clientId`.  If not found, throw a `NotFoundException`.
    *   Return:  The found `ClientDto`.

*   **updateClient(clientId: UUID, updateClientDto: UpdateClientDto): Promise<ClientDto>**:
    *   Implementation: Use Prisma Client to update the `Client` record with the given `clientId` using data from `updateClientDto`.  If not found, throw a `NotFoundException`.
    *   Return:  The updated `ClientDto`.

*   **deleteClient(clientId: UUID): Promise<void>**:
    *   Implementation: Use Prisma Client to delete the `Client` record with the given `clientId`. If not found, throw a `NotFoundException`.
    *   Return: `void` (or potentially a success message).


### PatientService:

*   **(Define similar service methods for Patients - `createPatient`, `getPatients`, `getPatientById`, `updatePatient`, `deletePatient`)**

    *   Use `PatientService` class.
    *   Implement logic analogous to `ClientService` methods, but for `Patient` entity and DTOs (`CreatePatientDto`, `UpdatePatientDto`, `PatientDto`).
    *   Adapt Prisma queries to work with `DimPatients` table.

---