# 73_UserRolesPermissions_Backend_Services.md

**Core Feature:** User Roles and Permissions (RBAC) - Backend Services (NestJS)

**Purpose:** Define the NestJS services for managing Roles, Permissions, User-Role assignments, and implementing RBAC logic, interacting with Prisma ORM.

**Instructions for AI Agent:**

*   **Generate NestJS service classes:** `RoleService`, `PermissionService`, `UserRoleService`, `RBACService` (or potentially combine RBAC logic into `AuthService` or a dedicated Guard utility service).
*   For each service method (corresponding to API endpoints in `72_UserRolesPermissions_Backend_API.md`):
    *   Method signature with parameters and return types (TypeScript).
    *   Prisma Client interactions for CRUD operations on `LookupRoles`, `LookupPermissions`, `RolePermissions` tables.
    *   Logic for managing role-permission assignments and user-role assignments.
    *   Implement logic in `RBACService` (or Guard) to retrieve user roles and permissions from the database (or potentially from JWT if roles/permissions are included there - *decided to retrieve from DB for each request for now*).
    *   Error handling (Prisma exceptions, HTTP exceptions).
    *   Logging.
    *   Mapping Prisma entities to DTOs.

*   **Implement RBAC Guard Logic in `RBACService` or `PermissionsGuard`:**
    *   Method to check if a user has a specific role.
    *   Method to check if a user has a specific permission.
    *   Method to retrieve all permissions for a user (based on their roles).
    *   Integrate with NestJS Reflector to read role/permission requirements from controllers/endpoints.

---

## Service Definitions:

### RoleService:

*   **createRole(createRoleDto: CreateRoleDto): Promise<RoleDto>**: Prisma `create` for `LookupRoles`.
*   **getRoles(): Promise<RoleDto[]>**: Prisma `findMany` for `LookupRoles`.
*   **getRoleById(roleId: UUID): Promise<RoleDto>**: Prisma `findUnique` for `LookupRoles` by `roleId`. `NotFoundException` if not found.
*   **updateRole(roleId: UUID, updateRoleDto: UpdateRoleDto): Promise<RoleDto>**: Prisma `update` for `LookupRoles`. `NotFoundException` if not found.
*   **deleteRole(roleId: UUID): Promise<void>**: Prisma `delete` for `LookupRoles`. `NotFoundException` if not found.

### PermissionService:

*   **getPermissions(): Promise<PermissionDto[]>**: Prisma `findMany` for `LookupPermissions`.  *(Initially, only listing, no CRUD for permissions themselves)*

### RolePermissionService (or methods within `RoleService`):

*   **assignPermissionsToRole(roleId: UUID, permissionIds: UUID[]): Promise<RoleDto>**:
    *   Implementation: Use Prisma to create records in `RolePermissions` table linking `roleId` to each `permissionId` in the `permissionIds` array.  Ensure no duplicates are created. Return the updated `RoleDto` with associated permissions.

*   **getRolePermissions(roleId: UUID): Promise<PermissionDto[]>**:
    *   Implementation: Use Prisma to find all `PermissionDto` records associated with a given `roleId` by querying `RolePermissions` and joining with `LookupPermissions`.

*   **removePermissionsFromRole(roleId: UUID, permissionIds: UUID[]): Promise<RoleDto>**:
    *   Implementation: Use Prisma to delete records from `RolePermissions` table where `roleId` matches and `permissionId` is in the `permissionIds` array. Return the updated `RoleDto` after removing permissions.

### UserRoleService:

*   **assignRoleToUser(assignRoleToUserDto: AssignRoleToUserDto): Promise<UserRoleAssignment | any>**: // Define `UserRoleAssignment` DTO if needed, or return User object
    *   Implementation: Use Prisma to create a relationship between a user (e.g., `CentralUsers`) and a role (e.g., `LookupRoles`).  Update the user record to include the assigned role (or create a separate UserRoles join table if many-to-many user-role relationship is needed later).  For now, assume one role per user for simplicity, and store `role_id` directly in `CentralUsers` table (add `role_id` FK to `CentralUsers` table if not already there).

*   **getUserRoles(userId: UUID): Promise<RoleDto[]>**: // Or Promise<RoleDto> if single role per user
    *   Implementation:  Use Prisma to fetch the `RoleDto` associated with a given `userId` by querying `CentralUsers` and joining with `LookupRoles` based on `role_id`.  Return an array of RoleDto even if single role is assumed for now, for potential future expansion.

*   **removeRoleFromUser(userId: UUID, roleId: UUID): Promise<void>**:
    *   Implementation: Use Prisma to remove the association between a user and a role.  Set `role_id` to `null` in `CentralUsers` table for the given `userId`.


### RBACService (or PermissionsGuard):

*   **hasRole(userId: UUID, roleName: string): Promise<boolean>**:
    *   Implementation:  Fetch user's roles using `UserRoleService.getUserRoles(userId)`. Check if any of the user's roles match the given `roleName`.

*   **hasPermission(userId: UUID, permissionName: string): Promise<boolean>**:
    *   Implementation:
        *   Fetch user's roles using `UserRoleService.getUserRoles(userId)`.
        *   For each role, fetch associated permissions using `RolePermissionService.getRolePermissions(roleId)`.
        *   Check if any of the user's permissions match the given `permissionName`.

*   **getPermissionsForUser(userId: UUID): Promise<PermissionDto[]>**:
    *   Implementation:
        *   Fetch user's roles using `UserRoleService.getUserRoles(userId)`.
        *   For each role, fetch associated permissions using `RolePermissionService.getRolePermissions(roleId)`.
        *   Combine all permissions (avoid duplicates if a user has multiple roles with overlapping permissions).
        *   Return the array of `PermissionDto` objects.

---