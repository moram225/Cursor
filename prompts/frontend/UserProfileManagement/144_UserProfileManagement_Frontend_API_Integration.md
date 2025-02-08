# 144_UserProfileManagement_Frontend_API_Integration.md

**Frontend Feature:** User Profile Management - Frontend API Integration

**Purpose:**

This file instructs the AI agent to integrate the frontend User Profile Management page and components with the backend User Profile Management API endpoints.  This will involve implementing API calls to fetch user profile data, update profile information, and change passwords.

**Instructions for AI Agent:**

1.  **Import API Client Functions:** In the relevant components and pages:

    *   **`ProfileSettingsPage.tsx` (`pages/settings/profile.tsx`):** Import the API client function to fetch user profile data (e.g., `userProfileApi.getProfile`) and potentially to update profile (though update is handled in the form component).
    *   **`UserProfileEditForm.tsx` (`src/components/settings/UserProfileEditForm.tsx`):** Import the API client function to update user profile data (e.g., `userProfileApi.updateProfile`).
    *   **`ChangePasswordForm.tsx` (`src/components/settings/ChangePasswordForm.tsx`):** Import the API client function to change password (e.g., `userProfileApi.changePassword`).

2.  **Implement API Calls and Data Handling:**

    *   **`ProfileSettingsPage.tsx` - Fetch User Profile Data:**
        *   In the `useEffect` hook within `ProfileSettingsPage`, replace the mock data fetching with an actual API call:
            ```javascript
            useEffect(() => {
                const fetchProfileData = async () => {
                    try {
                        const profileData = await userProfileApi.getProfile(); // Call API client function
                        setUserData(profileData);
                    } catch (error) {
                        console.error("Error fetching profile data:", error);
                        // Handle error - display error message to user (e.g., using a state variable and displaying an error message component)
                        // For now, just log the error.
                    }
                };

                if (isAuthenticated()) { // Only fetch if authenticated
                    fetchProfileData();
                }
            }, [isAuthenticated]);
            ```
        *   Ensure you handle potential errors during API calls (e.g., network errors, server errors). Display user-friendly error messages in the UI.

    *   **`UserProfileEditForm.tsx` - Update Profile:**
        *   In the `UserProfileEditForm` component, modify the `onSubmit` handler of the form:
            ```javascript
            const handleSubmit = async (event: React.FormEvent) => {
                event.preventDefault();
                try {
                    const updatedData = { firstName, lastName, phone }; // Data from form state
                    const response = await userProfileApi.updateProfile(updatedData); // Call API client function to update
                    // Handle success - display success message to user (e.g., set a success message state and display it)
                    console.log("Profile updated successfully:", response);
                    // Optionally, you might want to refetch profile data after successful update to reflect changes immediately, or update the local state directly with the response data.
                    setSuccessMessage("Profile updated successfully!");
                    setErrorMessage(null); // Clear any previous error message
                } catch (error) {
                    console.error("Error updating profile:", error);
                    // Handle error - display error message (e.g., set an error message state and display it using FormErrorMessage component)
                    setErrorMessage("Error updating profile. Please try again.");
                    setSuccessMessage(null); // Clear any previous success message
                }
            };
            ```
        *   Pass the `handleSubmit` function to the `<form onSubmit={handleSubmit}>` in the `UserProfileEditForm` component.
        *   Implement state variables (`successMessage`, `errorMessage`) to manage success and error messages and display them in the component using `FormErrorMessage` and a success message display (e.g., using a styled `Typography` or a dedicated `SuccessMessage` component if you want to create one later).

    *   **`ChangePasswordForm.tsx` - Change Password:**
        *   In the `ChangePasswordForm` component, modify the `onSubmit` handler:
            ```javascript
            const handleSubmit = async (event: React.FormEvent) => {
                event.preventDefault();
                if (newPassword !== confirmNewPassword) {
                    setErrorMessage("New passwords do not match.");
                    return;
                }
                try {
                    const passwordChangeData = { currentPassword, newPassword };
                    const response = await userProfileApi.changePassword(passwordChangeData); // Call API client function
                    // Handle success - display success message
                    console.log("Password changed successfully:", response);
                    setSuccessMessage("Password changed successfully!");
                    setErrorMessage(null);
                    // Optionally, clear the password input fields after successful change
                    setCurrentPassword('');
                    setNewPassword('');
                    setConfirmNewPassword('');

                } catch (error) {
                    console.error("Error changing password:", error);
                    // Handle error - display error message
                    setErrorMessage("Error changing password. Please check your current password and try again.");
                    setSuccessMessage(null);
                }
            };
            ```
        *   Pass the `handleSubmit` function to the `<form onSubmit={handleSubmit}>` in the `ChangePasswordForm` component.
        *   Implement state variables (`successMessage`, `errorMessage`) and message display as you did for `UserProfileEditForm`.
        *   Implement client-side password matching validation before calling the API.

3.  **Error Handling and User Feedback:**

    *   Ensure all API calls have proper error handling (using `.catch()` or `try...catch`).
    *   Display user-friendly error messages using the `FormErrorMessage` component for API call failures and validation errors.
    *   Display success messages to confirm successful profile updates and password changes.
    *   Consider adding loading/spinner indicators while API calls are in progress to improve user experience.

**File Modifications:**

*   `pages/settings/profile.tsx` (Modify `useEffect` to fetch profile data from API)
*   `src/components/settings/UserProfileEditForm.tsx` (Implement `onSubmit` handler to call API for profile update, handle success/error messages)
*   `src/components/settings/ChangePasswordForm.tsx` (Implement `onSubmit` handler to call API for password change, handle success/error messages, client-side password match validation)


**Instructions for AI Agent (Summary):**

*   Import relevant API client functions into `ProfileSettingsPage`, `UserProfileEditForm`, and `ChangePasswordForm` components.
*   Implement API calls to fetch user profile data in `ProfileSettingsPage` on mount.
*   Implement API calls to update profile data in `UserProfileEditForm` on form submission.
*   Implement API calls to change password in `ChangePasswordForm` on form submission, include client-side password match validation.
*   Implement error handling for all API calls and display user-friendly error messages.
*   Display success messages for successful updates and password changes.

**Next Steps:**

*   After implementing API integration, the User Profile Management page should be fully functional!
*   The next step is to thoroughly test the User Profile Management functionality:
    *   View profile information.
    *   Edit profile information (name, phone number).
    *   Change password (successful change, incorrect current password, password mismatch errors).
    *   Verify error handling for API failures.