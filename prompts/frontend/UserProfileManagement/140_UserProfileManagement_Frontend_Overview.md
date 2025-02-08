# 140_UserProfileManagement_Frontend_Overview.md

**Frontend Feature:** User Profile Management Frontend

**Purpose:**

This feature provides users with frontend interfaces to view and manage their own profile information and settings.  It will allow users to view their profile details, update editable information, and change their password, leveraging the backend API endpoints we defined earlier in the User Profile Management backend feature.

**User Stories:**

*   As a **User**, I want to be able to **access my profile settings** from within the application so I can manage my account.
*   As a **User**, I want to be able to **view my profile information** (name, email, contact details, role) in a clear and organized manner.
*   As a **User**, I want to be able to **edit my personal information** such as my name and phone number.
*   As a **User**, I want to be able to **change my password** to ensure account security.
*   As a **User**, I want to receive **clear feedback** when I successfully update my profile or change my password, and see appropriate error messages if there are issues.

**Key Pages:**

*   **Profile Settings Page (`/settings/profile`):**
    *   This will be the main page for users to manage their profile.  It will be accessible from within the application (e.g., via a navigation menu or dropdown in the header).
    *   **View Profile Section:**
        *   Display user's non-editable profile information (e.g., Email, Role - might depend on design).
        *   Display user's editable profile information (Name, Phone Number - and potentially other fields).

    *   **Edit Profile Section:**
        *   Form to edit editable profile fields (Name, Phone Number).
        *   Input fields for each editable field, pre-populated with current user data.
        *   "Save Changes" button to submit profile updates.
        *   Success/Error message display upon profile update.

    *   **Change Password Section:**
        *   Form to change password.
        *   Current Password input field.
        *   New Password input field.
        *   Confirm New Password input field.
        *   "Change Password" button to submit password change request.
        *   Success/Error message display upon password change.

**Key Components (React Components):**

*   **ProfileSettingsPage Component (`src/pages/settings/profile.tsx`):**  (This will be the Next.js page)
    *   Main page component for `/settings/profile`.
    *   Will contain the View Profile, Edit Profile, and Change Password sections/components.
    *   Needs to fetch the user's profile data on page load from the backend API (`/users/profile` - defined in `102_UserProfileManagement_Backend_API.md`).
    *   Will orchestrate the sub-components for displaying and managing profile information.
    *   Should be a protected route - only accessible to logged-in users (using the AuthContext we created).

*   **UserProfileView Component (`src/components/settings/UserProfileView.tsx`):**
    *   Purpose:  Displays the non-editable and some read-only profile information.
    *   Props:  `userData` (object containing user profile data).
    *   Display:  Use `Typography` and layout components (MUI `Box`, `Grid`) to present user details in a readable format.

*   **UserProfileEditForm Component (`src/components/settings/UserProfileEditForm.tsx`):**
    *   Purpose:  Form to allow users to edit their editable profile fields.
    *   Props: `initialUserData` (object to pre-populate the form with current data).
    *   Form Fields:  Use `FormInputField` components (from UI component library) for Name, Phone Number (and other editable fields).
    *   "Save Changes" Button: Use `FormButton` component.
    *   Form Logic: Manage form state, handle form submission, call backend API (`PUT /users/profile` - defined in `102_UserProfileManagement_Backend_API.md`) to update profile, handle success and error responses, display messages to the user.

*   **ChangePasswordForm Component (`src/components/settings/ChangePasswordForm.tsx`):**
    *   Purpose: Form to allow users to change their password.
    *   Form Fields:  Use `FormInputField` components for Current Password, New Password, Confirm New Password.
    *   "Change Password" Button: Use `FormButton` component.
    *   Form Logic:  Manage form state, handle form submission, call backend API (`POST /users/profile/change-password` - defined in `102_UserProfileManagement_Backend_API.md`) to change password, handle success and error responses, display messages to the user.

**Data Flow and API Interactions:**

*   **View Profile:**
    *   `ProfileSettingsPage` component, on page load (e.g., in `useEffect`), calls the backend API `GET /users/profile` (defined in `102_UserProfileManagement_Backend_API.md`) to fetch the current user's profile data.
    *   Data received from API is passed as props to `UserProfileView` and `UserProfileEditForm` components for display and initial form values.

*   **Update Profile:**
    *   `UserProfileEditForm` component, on form submission, sends updated profile data to the backend API `PUT /users/profile` (defined in `102_UserProfileManagement_Backend_API.md`).
    *   Backend API updates profile in the database and returns updated profile data (or success).
    *   Frontend handles success or error responses, displays messages to the user, and potentially updates the displayed profile information.

*   **Change Password:**
    *   `ChangePasswordForm` component, on form submission, sends current password, new password, and confirm new password to the backend API `POST /users/profile/change-password` (defined in `102_UserProfileManagement_Backend_API.md`).
    *   Backend API verifies current password, updates password in the database, and returns success or error.
    *   Frontend handles success or error responses, displays messages to the user.

**Instructions for AI Agent:**

*   Based on this overview and the UI library setup, proceed to create the React components for the User Profile Management frontend feature in `142_UserProfileManagement_Frontend_Components.md`.
*   Focus on creating the `UserProfileView`, `UserProfileEditForm`, and `ChangePasswordForm` components, and the main `ProfileSettingsPage` page component.
*   Utilize MUI Core and Joy UI components for layout, styling, and form elements.  Reuse the `FormInputField`, `FormButton`, and `FormErrorMessage` components we created for Authentication.
*   Ensure components are designed for reusability, maintainability, and good user experience.
*   After defining the components, proceed to create the Next.js page (`/settings/profile.tsx`) in `143_UserProfileManagement_Frontend_Pages.md`, using these components to build the User Profile Settings page and configure routing (protected route).

**Next Steps:**

*   After generating the components and page, we will have the basic frontend UI for User Profile Management in place.
*   The next steps will be to:
    *   Implement API integration in the form components to connect to the backend User Profile Management API endpoints.
    *   Implement form validation (client-side and potentially server-side).
    *   Implement authentication guard for the `/settings/profile` page to protect it.