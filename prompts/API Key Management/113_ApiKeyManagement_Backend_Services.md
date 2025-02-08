# 113_ApiKeyManagement_Backend_Services.md

**Core Feature:** API Key Management - Backend Services (NestJS)

**Purpose:** Define the NestJS services for managing Tenant API Keys, including generation, storage, retrieval, revocation, and validation, interacting with Prisma ORM and crypto libraries.

**Instructions for AI Agent:**

*   **Generate NestJS service class:** `ApiKeyService`.
*   For each service method (corresponding to API endpoints in `112_ApiKeyManagement_Backend_API.md`):
    *   Method signature with parameters and return types (TypeScript).
    *   Prisma Client interactions for CRUD operations on `TenantApiKeys` table.
    *   Implement logic in `ApiKeyService` for:
        *   Generating cryptographically secure random API keys using a library like `crypto` (Node.js built-in).
        *   Storing API keys in the database (initially store in plain text for simplicity, *but highlight the security risk and the need to hash or encrypt in production*).
        *   Retrieving API keys (excluding the key value itself in list/detail views for security, only return key value once on creation).
        *   Revoking API keys (setting `is_active` to `false` or deleting).
        *   Validating API keys:
            *   Retrieve API key from the database by `api_key_value`.
            *   Check if the key exists, is active (`is_active: true`), and has not expired (`expires_at` - if expiry is implemented).
            *   If valid, return an object containing `tenantId` and `userId` associated with the API key.
            *   If invalid, return `null`.
    *   Error handling (Prisma exceptions, HTTP exceptions).
    *   Logging.
    *   Mapping Prisma entities to DTOs (`ApiKeyDto`).

---

## Service Definitions:

### ApiKeyService:

*   **generateApiKey(): string**:
    *   Implementation: Use `crypto.randomBytes(32).toString('hex')` (or similar robust method) to generate a cryptographically secure random string for the API key.

*   **createApiKey(createApiKeyDto: CreateApiKeyDto, userId: UUID, tenantId: UUID): Promise<ApiKeyDto>**:
    *   Implementation:
        *   Generate a new API key value using `generateApiKey()`.
        *   Create a new record in `TenantApiKeys` table using Prisma `create` with data from `createApiKeyDto`, generated `api_key_value`, `userId`, and `tenantId`.
        *   Return the created `ApiKeyDto` (including the generated `api_key_value`).

*   **getApiKeys(tenantId: UUID): Promise<ApiKeyDto[]>**:
    *   Implementation: Prisma `findMany` for `TenantApiKeys` filtered by `tenant_id`. *Exclude `api_key_value` from the returned DTOs for security.*

*   **getApiKeyById(apiKeyId: UUID): Promise<ApiKeyDto>**:
    *   Implementation: Prisma `findUnique` for `TenantApiKeys` by `api_key_id`. *Exclude `api_key_value` from the returned DTO.* `NotFoundException` if not found.

*   **revokeApiKey(apiKeyId: UUID): Promise<void>**:
    *   Implementation: Prisma `delete` for `TenantApiKeys` by `api_key_id`.  Or, alternatively, update `is_active` to `false` instead of deleting for audit trail purposes.  For now, use `delete`. `NotFoundException` if not found.

*   **validateApiKey(apiKey: string): Promise<{ tenantId: UUID; userId: UUID } | null>**:
    *   Implementation:
        *   Prisma `findUnique` for `TenantApiKeys` where `api_key_value` is the provided `apiKey`, and `is_active` is `true`, and `expires_at` is either null or in the future (if expiry is implemented).
        *   If a valid API key is found, return an object containing `tenantId` and `user_id` from the retrieved `TenantApiKey` record.
        *   If no valid API key is found, return `null`.