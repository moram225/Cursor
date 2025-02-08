# 72_UserRolesPermissions_Backend_API.md

**Core Feature:** User Roles and Permissions (RBAC) - Backend API Endpoints (NestJS)

**Purpose:** Define the RESTful API endpoints for managing Roles, Permissions, and User-Role assignments in the backend NestJS application.  Also define how RBAC will be enforced on existing API endpoints.

**Instructions for AI Agent:**

*   **Generate NestJS controller code** with the API endpoints defined below.
*   Use `/roles`, `/permissions`, and `/user-roles` as base paths for controllers.
*   For each endpoint, generate:
    *   NestJS Controller decorator (`@Controller`)
    *   Route path decorators (`@Get`, `@Post`, `@Put`, `@Delete`)
    *   Method signature with parameters and types (TypeScript).
    *   Basic method body calling service methods (placeholders to services in `73_UserRolesPermissions_Backend_Services.md`).
    *   Appropriate HTTP status codes.
    *   Authentication (`@UseGuards(JwtAuthGuard)`). *All RBAC management endpoints should be restricted to SuperAdmin role.*
    *   Basic validation using NestJS `ValidationPipe` and DTOs.

*   **Define RBAC Enforcement Strategy:**
    *   Create a custom NestJS Guard (e.g., `RolesGuard`, `PermissionsGuard`) that will be used to protect API endpoints.
    *   This guard should:
        *   Extract user information (roles, permissions) from the JWT token (or retrieve from database based on user ID if not in JWT - *for now, assume roles/permissions will be retrieved from database for each request to keep JWT smaller, optimization can be considered later*).
        *   Check if the user has the required role or permission to access the endpoint.
        *   Use NestJS Reflector to get metadata about required roles/permissions for each endpoint (using custom decorators like `@Roles('Admin', 'Manager')`, `@Permissions('read:patient', 'update:patient')`).
        *   Return `true` to allow access or `false` to deny access (throw `UnauthorizedException` or `ForbiddenException`).
    *   Define custom decorators `@Roles` and `@Permissions` to specify required roles and permissions for API endpoints.

---

## API Endpoints:

### Role Endpoints (`/roles`):

*   **POST /roles**: Create a new Role.
    *   Method: `POST`
    *   Request Body: `CreateRoleDto` (Define DTO with fields from `LookupRoles` table - role_name, description - validation).  *Restrict access to SuperAdmin only - `@Roles('SuperAdmin')`.*
    *   Response: `RoleDto`, 201 Created.

*   **GET /roles**: Get a list of Roles.
    *   Method: `GET`
    *   Response: `RoleDto[]`, 200 OK.  *Restrict access to SuperAdmin or authorized roles - `@Roles('SuperAdmin', 'Admin', 'RoleManager')` - determine appropriate roles for role management access.*

*   **GET /roles/:roleId**: Get a specific Role by ID.
    *   Method: `GET`
    *   Path Parameter: `roleId` (UUID)
    *   Response: `RoleDto`, 200 OK. *Restrict access similar to GET /roles.*

*   **PUT /roles/:roleId**: Update an existing Role.
    *   Method: `PUT`
    *   Path Parameter: `roleId` (UUID)
    *   Request Body: `UpdateRoleDto` (Partial updates, validation). *Restrict access to SuperAdmin only - `@Roles('SuperAdmin')`.*
    *   Response: `RoleDto`, 200 OK.

*   **DELETE /roles/:roleId**: Delete a Role.
    *   Method: `DELETE`
    *   Path Parameter: `roleId` (UUID)
    *   Response: 204 No Content. *Restrict access to SuperAdmin only - `@Roles('SuperAdmin')`.*


### Permission Endpoints (`/permissions`):

*   **GET /permissions**: Get a list of all Permissions.
    *   Method: `GET`
    *   Response: `PermissionDto[]`, 200 OK. *Restrict access to SuperAdmin or authorized roles - similar to GET /roles.*
    *   *Permissions are likely pre-defined in code or seed data, so creation/update/delete API endpoints might not be needed initially.  Focus on listing permissions for now.*

### Role-Permission Assignment Endpoints (`/roles/:roleId/permissions`):

*   **POST /roles/:roleId/permissions**: Assign Permissions to a Role.
    *   Method: `POST`
    *   Path Parameter: `roleId` (UUID)
    *   Request Body: `AssignPermissionsToRoleDto` (Define DTO with an array of `permission_ids` - UUID[], validation). *Restrict access to SuperAdmin only - `@Roles('SuperAdmin')`.*
    *   Response: `RoleDto` (updated role with assigned permissions), 200 OK.

*   **GET /roles/:roleId/permissions**: Get Permissions assigned to a specific Role.
    *   Method: `GET`
    *   Path Parameter: `roleId` (UUID)
    *   Response: `PermissionDto[]` (array of permissions assigned to the role), 200 OK. *Restrict access similar to GET /roles.*

*   **DELETE /roles/:roleId/permissions**: Remove Permissions from a Role.
    *   Method: `DELETE`
    *   Path Parameter: `roleId` (UUID)
    *   Request Body: `RemovePermissionsFromRoleDto` (Define DTO with an array of `permission_ids` - UUID[], validation). *Restrict access to SuperAdmin only - `@Roles('SuperAdmin')`.*
    *   Response: `RoleDto` (updated role after removing permissions), 200 OK.


### User-Role Assignment Endpoints (`/user-roles` or `/central-users/:userId/roles` - choose one, `/user-roles` might be more general for future tenant users):

*   **POST /user-roles**: Assign a Role to a User.
    *   Method: `POST`
    *   Request Body: `AssignRoleToUserDto` (Define DTO with `user_id` and `role_id` - UUIDs, validation). *Restrict access to SuperAdmin only - `@Roles('SuperAdmin')`.*
    *   Response:  User object (or UserRole assignment object), 201 Created.

*   **GET /user-roles/users/:userId**: Get Roles assigned to a specific User.
    *   Method: `GET`
    *   Path Parameter: `userId` (UUID)
    *   Response: `RoleDto[]` (array of roles assigned to the user), 200 OK. *Restrict access to SuperAdmin or user viewing their own roles (self-lookup - `@Roles('SuperAdmin', 'Self')` - need to define 'Self' role handling or logic).*

*   **DELETE /user-roles/users/:userId/roles/:roleId**: Remove a Role from a User.
    *   Method: `DELETE`
    *   Path Parameters: `userId` (UUID), `roleId` (UUID)
    *   Response: 204 No Content. *Restrict access to SuperAdmin only - `@Roles('SuperAdmin')`.*

---