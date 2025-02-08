# 112_ApiKeyManagement_Backend_API.md

**Core Feature:** API Key Management - Backend API Endpoints (NestJS)

**Purpose:** Define the RESTful API endpoints for managing Tenant API Keys in the backend NestJS application.  Also define the API Key authentication guard.

**Instructions for AI Agent:**

*   **Generate NestJS controller code** with the API endpoints defined below.
*   Use `/api-keys` as the base path for the controller.
*   For each endpoint, generate:
    *   NestJS Controller decorator (`@Controller`) - with `/api-keys` base path.
    *   Route path decorators (`@Get`, `@Post`, `@Delete`) - No PUT for API keys, revocation is deletion.
    *   Method signature with parameters and types (TypeScript).
    *   Basic method body calling service methods (placeholders to services in `113_ApiKeyManagement_Backend_Services.md`).
    *   Appropriate HTTP status codes.
    *   Authentication (`@UseGuards(JwtAuthGuard)`, `@Roles('TenantAdmin')` - restrict API key management to Tenant Admin role).
    *   Basic validation using NestJS `ValidationPipe` and DTOs.

*   **Define API Key Authentication Guard (`ApiKeyAuthGuard`):**
    *   Create a custom NestJS Guard `ApiKeyAuthGuard`.
    *   This guard should:
        *   Extract the API key from the `Authorization` header (e.g., `Authorization: Api-Key <your_api_key>`).
        *   Call `ApiKeyService.validateApiKey(apiKey)` to validate the key against the database and retrieve associated tenant and user information if valid.
        *   If the API key is valid:
            *   Attach the tenant ID and user ID (associated with the API key) to the request object (e.g., `request.tenantId`, `request.apiKeyUserId`).  This will allow downstream controllers/services to access the tenant context from the API key.
            *   Return `true` to allow access.
        *   If the API key is invalid or missing:
            *   Return `false` to deny access (throw `UnauthorizedException` or `ForbiddenException`).

---

## API Endpoints (`/api-keys`):

*   **POST /api-keys**: Generate a new API Key.
    *   Method: `POST`
    *   Request Body: `CreateApiKeyDto` (Define DTO with `description` - optional, validation). *Restrict access to TenantAdmin role - `@Roles('TenantAdmin')`.*
    *   Response: `ApiKeyDto` (including the generated `api_key_value` - *consider security implications of returning the key value in response - for initial version, okay, but in production, might return only once and then store securely*), 201 Created.

*   **GET /api-keys**: Get a list of API Keys for the current tenant.
    *   Method: `GET`
    *   Response: `ApiKeyDto[]` (excluding `api_key_value` for security in list view, only return in create response), 200 OK. *Restrict access to TenantAdmin role - `@Roles('TenantAdmin')`.*

*   **GET /api-keys/:apiKeyId**: Get details of a specific API Key.
    *   Method: `GET`
    *   Path Parameter: `apiKeyId` (UUID)
    *   Response: `ApiKeyDto` (excluding `api_key_value` for security, unless specifically needed - for now, exclude from GET by ID as well), 200 OK. *Restrict access to TenantAdmin role - `@Roles('TenantAdmin')`.*

*   **DELETE /api-keys/:apiKeyId**: Revoke (delete/deactivate) an API Key.
    *   Method: `DELETE`
    *   Path Parameter: `apiKeyId` (UUID)
    *   Response: 204 No Content. *Restrict access to TenantAdmin role - `@Roles('TenantAdmin')`.*

---

**Example of using ApiKeyAuthGuard on other API endpoints (for future use):**

```typescript
import { Controller, Get, UseGuards } from '@nestjs/common';
import { ApiKeyAuthGuard } from './api-key-auth.guard'; // Assuming guard is in the same directory

@Controller('external-api')
export class ExternalApiController {

  @Get('data')
  @UseGuards(ApiKeyAuthGuard) // Apply API Key authentication guard
  getData(@Request() req): any { // Request object will have tenantId and apiKeyUserId attached by the guard
    const tenantId = req.tenantId;
    const apiKeyUserId = req.apiKeyUserId;
    // ... access tenant-specific data using tenantId and perform actions authorized for apiKeyUserId ...
    return { message: `Data for tenant ${tenantId} accessed via API Key by user ${apiKeyUserId}` };
  }
}