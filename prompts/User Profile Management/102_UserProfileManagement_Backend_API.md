# 102_UserProfileManagement_Backend_API.md

**Core Feature:** User Profile Management - Backend API Endpoints (NestJS)

**Purpose:** Define the RESTful API endpoints for managing User Profiles in the backend NestJS application.

**Instructions for AI Agent:**

*   **Generate NestJS controller code** with the API endpoints defined below.
*   Use `/users/profile` as the base path for the controller.
*   For each endpoint, generate:
    *   NestJS Controller decorator (`@Controller`) - with `/users/profile` base path.
    *   Route path decorators (`@Get`, `@Put`, `@Post`) - No DELETE for user profile itself, only viewing and updating. POST for password change can be considered.
    *   Method signature with parameters and types (TypeScript).
    *   Basic method body calling service methods (placeholders to services in `103_UserProfileManagement_Backend_Services.md`).
    *   Appropriate HTTP status codes.
    *   Authentication (`@UseGuards(JwtAuthGuard)`). *All profile management endpoints should be accessible to authenticated users to manage their own profile.*
    *   Basic validation using NestJS `ValidationPipe` and DTOs.

---

## API Endpoints (`/users/profile`):

*   **GET /users/profile**: Get the current user's profile.
    *   Method: `GET`
    *   Response: `UserProfileDto` (Define DTO with relevant user profile fields from `CentralUsers` table - name, email, phone, etc., and communication preferences), 200 OK.
    *   Description: Retrieves the profile information of the currently authenticated user.  *Authorization: Authenticated User - access to own profile.*

*   **PUT /users/profile**: Update the current user's profile.
    *   Method: `PUT`
    *   Request Body: `UpdateUserProfileDto` (Define DTO with editable profile fields - name, phone, communication preferences - validation).
    *   Response: `UserProfileDto` (updated profile), 200 OK.
    *   Description: Updates the profile information of the currently authenticated user. *Authorization: Authenticated User - update own profile.*

*   **POST /users/profile/change-password**: Change the current user's password.
    *   Method: `POST`
    *   Request Body: `ChangePasswordDto` (Define DTO with `oldPassword` and `newPassword` - validation, password strength requirements).
    *   Response:  Success message (e.g., `{ message: 'Password changed successfully' }`), 200 OK.  Or potentially return the updated `UserProfileDto` if other profile fields are updated during password change (though less likely).
    *   Description: Allows the currently authenticated user to change their password. *Authorization: Authenticated User - change own password.*

*   **(Potentially consider separate endpoints for managing communication preferences if they become more complex - e.g., `/users/profile/communication-preferences` - but for now, keep it within `/users/profile` for simplicity.)*

---