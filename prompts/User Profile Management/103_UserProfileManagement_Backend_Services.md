# 103_UserProfileManagement_Backend_Services.md

**Core Feature:** User Profile Management - Backend Services (NestJS)

**Purpose:** Define the NestJS services for managing User Profiles, including retrieving profile data, updating profile information, and changing passwords, interacting with Prisma ORM and potentially authentication services.

**Instructions for AI Agent:**

*   **Generate NestJS service class:** `UserProfileService` (or `UserService` and extend existing `AuthService` if password change logic is closely tied to authentication).
*   For each service method (corresponding to API endpoints in `102_UserProfileManagement_Backend_API.md`):
    *   Method signature with parameters and return types (TypeScript).
    *   Prisma Client interactions for retrieving and updating `CentralUsers` table data.
    *   Logic for password change (using password hashing from AuthService - reuse existing password hashing logic).
    *   Validation of input data.
    *   Error handling (Prisma exceptions, HTTP exceptions).
    *   Logging.
    *   Mapping Prisma entities to DTOs (`UserProfileDto`).

---

## Service Definitions:

### UserProfileService (or UserService):

*   **getUserProfile(userId: UUID): Promise<UserProfileDto>**:
    *   Implementation: Use Prisma `findUnique` to retrieve user data from `CentralUsers` table by `userId`.  Map to `UserProfileDto`.  Include relevant profile fields and communication preferences.

*   **updateUserProfile(userId: UUID, updateUserProfileDto: UpdateUserProfileDto): Promise<UserProfileDto>**:
    *   Implementation: Use Prisma `update` to update fields in `CentralUsers` table for the given `userId` with data from `UpdateUserProfileDto`. Validate input data. Map updated entity to `UserProfileDto`.

*   **changePassword(userId: UUID, changePasswordDto: ChangePasswordDto): Promise<void>**: // Or Promise<UserProfileDto> if returning updated profile
    *   Implementation:
        *   Retrieve current user from database by `userId`.
        *   **Verify `oldPassword`**: Use password hashing service (from AuthService) to compare `changePasswordDto.oldPassword` with the stored hashed password of the user.  Throw `UnauthorizedException` if `oldPassword` is incorrect.
        *   **Hash `newPassword`**: Use password hashing service to hash `changePasswordDto.newPassword`.
        *   Update the `password` field in `CentralUsers` table for the given `userId` with the newly hashed password.
        *   *(Consider if any other profile fields should be updated as part of password change process - e.g., password last changed date).*
        *   Return void or updated `UserProfileDto`.

---