# 114_ApiKeyManagement_Backend_Controllers.md

**Backend Feature:** API Key Management - Backend Controllers (NestJS)

**Purpose:**

This file defines the NestJS controllers for the API Key Management backend feature. It instructs the AI agent to create the controllers that will handle incoming HTTP requests for API key related operations, such as generating new API keys, retrieving API keys, revoking/deactivating API keys, and potentially rotating API keys. These controllers will be responsible for request validation, routing requests to the appropriate services, and returning HTTP responses.

**Instructions for AI Agent:**

1.  **Create NestJS Controller (`ApiKeyManagement.controller.ts`):**
    *   Generate a NestJS controller file named `apiKeyManagement.controller.ts` within the `src/api-key-management/controllers/` directory. Create the `controllers` directory if it doesn't exist within `src/api-key-management/`.
    *   Decorate the controller class with `@Controller('api-keys')` to define the base route for all API key management endpoints as `/api-keys`.
    *   Ensure the controller is properly decorated with `@UseGuards(JwtAuthGuard, RolesGuard)` and `@Roles(UserRole.ADMIN, UserRole.SUPER_ADMIN)` to protect these endpoints and restrict access to users with `ADMIN` or `SUPER_ADMIN` roles. Import `JwtAuthGuard`, `RolesGuard`, and `Roles` decorator, and `UserRole` enum from appropriate locations (as defined in previous Authentication and RBAC setup).

2.  **Implement Controller Methods and Endpoints:**  Define the following controller methods to handle different API key operations. For each method:
    *   Use appropriate HTTP method decorators (`@Post`, `@Get`, `@Put`, `@Delete`).
    *   Define specific route paths within the `@Controller('api-keys')` base route (e.g., `/generate`, `/:keyId`, `/revoke/:keyId`).
    *   Inject the `ApiKeyManagementService` into the controller constructor to access the service methods for business logic.
    *   Implement request parameter extraction (using `@Body`, `@Param`, `@Query` decorators as needed).
    *   Call the corresponding service method in the `ApiKeyManagementService`.
    *   Return the result from the service method. NestJS automatically handles serialization and HTTP response formatting.
    *   Use `@HttpCode` decorator to specify explicit HTTP status codes for successful operations (e.g., `HttpStatus.CREATED` for successful creation, `HttpStatus.OK` for successful retrieval/update/delete).
    *   Implement basic validation using NestJS built-in validation pipes (e.g., `ValidationPipe`). Define Data Transfer Objects (DTOs) for request bodies (if not already created - check `112_ApiKeyManagement_Backend_API.md` and create DTOs in `src/api-key-management/dtos/` if needed) and use `@Body(ValidationPipe)` to apply validation.

    *   **`generateApiKey` (`POST /api-keys/generate`):**
        *   Purpose: Endpoint to generate a new API key for a tenant.
        *   Decorator: `@Post('generate')`
        *   Request Body:  Expect a request body with necessary information for API key generation. Define a DTO `GenerateApiKeyRequestDto` (if not already defined) with fields like `tenantId` (or `tenantIdentifier`), `description`, `expiryDate` (optional). Use `@Body(ValidationPipe) generateApiKeyRequestDto: GenerateApiKeyRequestDto`.
        *   Service Call: Call `apiKeyManagementService.generateApiKey(generateApiKeyRequestDto)`.
        *   HTTP Status Code: `@HttpCode(HttpStatus.CREATED)` (201 Created).
        *   Return:  Return the newly generated `ApiKey` object (or DTO).

    *   **`getAllApiKeysForTenant` (`GET /api-keys`):**
        *   Purpose: Endpoint to retrieve all API keys for the requesting tenant (or potentially all keys for admin/super-admin - *clarify tenant context* - for now, assume for requesting tenant).
        *   Decorator: `@Get()`
        *   Request:  Optionally, accept query parameters for pagination, filtering, etc. (e.g., pagination: `@Query('page', ParseIntPipe) page = 1, @Query('limit', ParseIntPipe) limit = 10`).
        *   Service Call: Call `apiKeyManagementService.getAllApiKeysForTenant(tenantId, paginationOptions)`.  *Need to determine how to get `tenantId` - from request context/user info or request body/parameter? Assume from request context based on JWT for now - may need to adjust based on multi-tenancy setup.*
        *   HTTP Status Code: `@HttpCode(HttpStatus.OK)` (200 OK).
        *   Return:  Return an array of `ApiKey` objects (or DTOs).

    *   **`getApiKeyById` (`GET /api-keys/:keyId`):**
        *   Purpose: Endpoint to retrieve a specific API key by its ID.
        *   Decorator: `@Get(':keyId')`
        *   Path Parameter: `@Param('keyId', ParseIntPipe) keyId: number`. Use `ParseIntPipe` for validation.
        *   Service Call: Call `apiKeyManagementService.getApiKeyById(keyId)`.
        *   HTTP Status Code: `@HttpCode(HttpStatus.OK)` (200 OK).
        *   Return: Return the `ApiKey` object (or DTO).

    *   **`revokeApiKey` (`PUT /api-keys/revoke/:keyId`):**
        *   Purpose: Endpoint to revoke (deactivate) an API key.
        *   Decorator: `@Put('revoke/:keyId')`
        *   Path Parameter: `@Param('keyId', ParseIntPipe) keyId: number`.
        *   Service Call: Call `apiKeyManagementService.revokeApiKey(keyId)`.
        *   HTTP Status Code: `@HttpCode(HttpStatus.OK)` (200 OK) - or `@HttpCode(HttpStatus.NO_CONTENT)` (204 No Content) if nothing is returned in the response body. Choose `@HttpCode(HttpStatus.OK)` and return the updated `ApiKey` object for now.
        *   Return: Return the updated `ApiKey` object (or DTO) after revocation.

    *   **`rotateApiKeySecret` (`PUT /api-keys/rotate/:keyId`):**
        *   Purpose: Endpoint to rotate the secret key of an API key (generate a new secret, invalidate the old one).
        *   Decorator: `@Put('rotate/:keyId')`
        *   Path Parameter: `@Param('keyId', ParseIntPipe) keyId: number`.
        *   Service Call: Call `apiKeyManagementService.rotateApiKeySecret(keyId)`.
        *   HTTP Status Code: `@HttpCode(HttpStatus.OK)` (200 OK).
        *   Return: Return the updated `ApiKey` object (or DTO) with the new secret (consider security implications of returning secret - maybe return masked secret or just confirmation). *For now, return updated ApiKey object*.

    *   **`deleteApiKey` (`DELETE /api-keys/:keyId`):**
        *   Purpose: Endpoint to permanently delete an API key.
        *   Decorator: `@Delete(':keyId')`
        *   Path Parameter: `@Param('keyId', ParseIntPipe) keyId: number`.
        *   Service Call: Call `apiKeyManagementService.deleteApiKey(keyId)`.
        *   HTTP Status Code: `@HttpCode(HttpStatus.OK)` (200 OK) - or `@HttpCode(HttpStatus.NO_CONTENT)` (204 No Content). Choose `@HttpCode(HttpStatus.OK)` and return a success message for now.
        *   Return: Return a success message (e.g., `{ message: 'API key deleted successfully' }`).


3.  **Dependency Injection:** Ensure `ApiKeyManagementController` depends on `ApiKeyManagementService` and inject it using constructor injection.  Make sure `ApiKeyManagementService` is properly decorated with `@Injectable()` in its definition file.

4.  **Module Registration:** Ensure `ApiKeyManagementController` is registered in the `ApiKeyManagementModule` in `apiKeyManagement.module.ts` within the `controllers` array.

**File Structure:**

*   `src/api-key-management/controllers/apiKeyManagement.controller.ts`
*   (Ensure `src/api-key-management/services/apiKeyManagement.service.ts` and `src/api-key-management/api-key-management.module.ts` exist from previous steps)
*   (Ensure DTOs in `src/api-key-management/dtos/` directory exist if defined for request bodies)

**Instructions for AI Agent (Summary):**

*   Create `ApiKeyManagementController` in `src/api-key-management/controllers/apiKeyManagement.controller.ts`.
*   Implement controller methods for `generateApiKey`, `getAllApiKeysForTenant`, `getApiKeyById`, `revokeApiKey`, `rotateApiKeySecret`, `deleteApiKey` with appropriate decorators, route paths, parameter extraction, and service calls.
*   Apply `@UseGuards(JwtAuthGuard, RolesGuard)` and `@Roles(UserRole.ADMIN, UserRole.SUPER_ADMIN)` for authorization.
*   Use `ValidationPipe` for request body validation with DTOs (if defined).
*   Return appropriate HTTP status codes and response bodies.
*   Implement dependency injection for `ApiKeyManagementService`.
*   Register the controller in `ApiKeyManagementModule`.

**Next Steps:**

*   After generating the controllers, the backend API endpoints for API Key Management should be defined and ready to handle requests.
*   The next steps would be to test these API endpoints using tools like Postman or Insomnia to ensure they are working correctly and interacting with the service logic as expected.