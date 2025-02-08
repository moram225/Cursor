# 110_ApiKeyManagement_Overview.md

**Core Feature:** API Key Management

**Purpose:** This feature enables root users (tenant administrators) to generate and manage API keys for their tenant. These API keys will allow authorized external applications to securely access the Veterinary PMS API, facilitating integrations and data exchange.  API key management is essential for providing controlled and secure programmatic access to tenant data.

**User Stories:**

*   As a **Root User (Tenant Administrator)**, I want to be able to **generate API keys** for my tenant so that I can authorize external applications to access the Veterinary PMS API.
*   As a **Root User (Tenant Administrator)**, I want to be able to **view a list of generated API keys** for my tenant, along with their descriptions and creation dates.
*   As a **Root User (Tenant Administrator)**, I want to be able to **revoke (delete/deactivate) API keys** if they are no longer needed or if they are compromised.
*   As a **Root User (Tenant Administrator)**, I want to be able to **add a description to each API key** to easily identify its purpose and the application it's intended for.
*   *(Future Enhancement)* As a **Root User (Tenant Administrator)**, I want to be able to **set an expiry date for API keys** for enhanced security.
*   *(Future Enhancement)* As a **Root User (Tenant Administrator)**, I want to be able to **control the permissions associated with each API key** to limit access to specific API resources (beyond tenant-level access).

**Key Functionalities:**

*   **API Key Generation:**
    *   Allow root users to generate new API keys.
    *   Generate cryptographically secure and unique API keys.
    *   Option to add a description/purpose to each API key.
    *   *(Future Enhancement)* Option to set an expiry date for API keys.

*   **API Key Listing:**
    *   Allow root users to view a list of API keys generated for their tenant.
    *   Display key information:  masked key value (for security), description, creation date, expiry date (if set), status (active/revoked).

*   **API Key Revocation:**
    *   Allow root users to revoke (deactivate or delete) API keys.
    *   Once revoked, an API key should no longer be valid for authentication.

*   **API Key Authentication:**
    *   Implement API key-based authentication for API requests from external applications.
    *   Verify API key validity against the database on each API request.
    *   Identify the tenant and potentially the root user associated with the API key for context and authorization.

**Database Tables (Relevant Entities):**

*   **TenantApiKeys**: Stores API key information.
    *   `api_key_id` (UUID, Primary Key)
    *   `tenant_id` (UUID, Foreign Key to `DimTenants`)
    *   `user_id` (UUID, Foreign Key to `CentralUsers` - the root user who generated the key)
    *   `api_key_value` (String, the actual API key - *consider hashing or encrypting this value in the future for enhanced security*)
    *   `description` (String, Optional)
    *   `created_at` (Timestamp, default current timestamp)
    *   `expires_at` (Timestamp, Optional)
    *   `is_active` (Boolean, default true, to allow deactivation instead of deletion)

**Instructions for AI Agent:**

*   Based on this overview and the database schema, specifically for tenant and central user entities, proceed to create the Backend API definitions in `112_ApiKeyManagement_Backend_API.md`.
*   Focus on creating API endpoints for:
    *   Generating new API keys (`POST /api-keys`).
    *   Listing API keys for a tenant (`GET /api-keys`).
    *   Retrieving a specific API key (`GET /api-keys/:apiKeyId`).
    *   Revoking/deleting an API key (`DELETE /api-keys/:apiKeyId`).
*   Design an API Key authentication mechanism (NestJS Guard) to validate API keys presented in request headers (e.g., `Authorization: Api-Key <your_api_key>`).
*   Ensure API design follows RESTful principles, uses appropriate HTTP methods and status codes, and includes authentication (for API key management endpoints - restrict to Root User/Tenant Admin roles) and validation.
*   After defining the API, generate the corresponding NestJS backend services in `113_ApiKeyManagement_Backend_Services.md`. These services will handle API key generation, storage, retrieval, revocation, and validation logic.