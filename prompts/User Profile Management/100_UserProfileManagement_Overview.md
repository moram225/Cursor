# 100_UserProfileManagement_Overview.md

**Core Feature:** User Profile Management

**Purpose:** This feature allows users to manage their own profile information and preferences within the Veterinary PMS. It empowers users to control aspects of their account and personalize their experience within the application.  This feature is essential for user convenience, personalization, and potentially for managing communication preferences and other user-specific settings.

**User Stories:**

*   As a **User**, I want to be able to **view my profile information**, including my name, email, contact details, and assigned role.
*   As a **User**, I want to be able to **update my personal information**, such as my name, phone number, and potentially address (if relevant).
*   As a **User**, I want to be able to **change my password** to maintain account security.
*   As a **User**, I want to be able to **manage my communication preferences**, such as preferred channels for receiving notifications (SMS, Email - link to Communication Tools feature).
*   *(Future Enhancement)* As a **User**, I want to be able to customize some basic UI preferences (e.g., theme, language - more relevant when frontend is developed).

**Key Functionalities:**

*   **View Profile:**
    *   Allow users to view their own profile details (name, email, contact info, role - potentially more details from `CentralUsers` table).

*   **Update Profile:**
    *   Allow users to update editable profile fields (name, phone number, potentially address, communication preferences).
    *   Validate updated profile data.

*   **Change Password:**
    *   Allow users to securely change their password.
    *   Implement password strength validation and password hashing (already in place from authentication feature, but ensure it's used here).

*   **Communication Preference Management:**
    *   Allow users to set their preferred communication channels for different types of notifications (SMS, Email - linking to Notification Types from Communication Tools).
    *   Store user communication preferences.

**Database Tables (Relevant Entities):**

*   **CentralUsers**: Stores user profile information (already existing table).
*   **LookupNotificationTypes**: For linking to communication preferences (already existing table).
*   *Potentially a new table `UserSettings` if settings become more complex and need separate storage - for now, consider storing basic profile info directly in `CentralUsers` and communication preferences as related lookups or fields within `CentralUsers`.*

**Instructions for AI Agent:**

*   Based on this overview and the database schema defined in `10a_Database_Central_Schema.md` and `10e_Database_Tenant_Lookup_Schema.md`, proceed to create the Backend API definitions in `102_UserProfileManagement_Backend_API.md`.
*   Focus on creating API endpoints for:
    *   Retrieving the current user's profile.
    *   Updating the current user's profile information.
    *   Changing the current user's password.
    *   Managing the current user's communication preferences.
*   Ensure API endpoints are secure and only allow users to manage *their own* profile data (authentication required, authorization to ensure user can only access their own profile).
*   Ensure API design follows RESTful principles, uses appropriate HTTP methods and status codes, and includes authentication and validation.
*   After defining the API, generate the corresponding NestJS backend services in `103_UserProfileManagement_Backend_Services.md`. These services will handle profile data retrieval, updates, password changes, and communication preference management.