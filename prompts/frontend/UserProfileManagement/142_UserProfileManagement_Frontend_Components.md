# 142_UserProfileManagement_Frontend_Components.md

**Frontend Feature:** User Profile Management - React Components

**Purpose:**

This file defines the React components for the User Profile Management frontend feature. It instructs the AI agent to generate these components using MUI Core and Joy UI, focusing on displaying profile information, providing edit forms, and handling password changes. We will reuse the `FormInputField`, `FormButton`, and `FormErrorMessage` components we created earlier.

**Instructions for AI Agent:**

1.  **Create React Functional Components:** Generate the following React functional components using TypeScript and JSX.

2.  **Utilize UI Libraries:**
    *   **MUI Core Components for Layout & Styling:** Primarily use MUI Core components for structuring the layout of the Profile Settings page and its sections. Use components like `Box`, `Grid`, `Typography`, `Card`, `CardContent` for layout, information display, and visual grouping.
    *   **Reuse UI Components:** Reuse the `FormInputField`, `FormButton`, and `FormErrorMessage` components from `src/components/ui/` that we created for the Authentication feature.
    *   **Joy UI (Minimal - if needed for specific styling):**  Use Joy UI sparingly if you find specific components or styling utilities that enhance the visual presentation, but primarily rely on MUI Core for consistency and ease of integration with existing theme.

3.  **Implement Component Structure and Basic Display:**

    *   **`UserProfileView` Component (`src/components/settings/UserProfileView.tsx`):**
        *   Purpose: Displays read-only user profile information.
        *   Props: `userData: UserProfileDto` (Assume a DTO interface for UserProfile with fields like `firstName`, `lastName`, `email`, `phone`, `role`).
        *   Structure:
            *   Use a `Card` or `Box` to contain the information.
            *   Display each profile field using `Typography` components with labels (e.g., "Name:", "Email:", "Role:").
            *   Organize fields using `Grid` or `Stack` for layout.
            *   Example Display:
                ```jsx
                <Card>
                    <CardContent>
                        <Typography variant="h6" gutterBottom>Profile Information</Typography>
                        <Grid container spacing={2}>
                            <Grid item xs={12} sm={6}>
                                <Typography variant="subtitle1">Name:</Typography>
                                <Typography>{userData.firstName} {userData.lastName}</Typography>
                            </Grid>
                            <Grid item xs={12} sm={6}>
                                <Typography variant="subtitle1">Email:</Typography>
                                <Typography>{userData.email}</Typography>
                            </Grid>
                            {/* ... similar Grid items for other fields (role, phone, etc.) ... */}
                        </Grid>
                    </CardContent>
                </Card>
                ```

    *   **`UserProfileEditForm` Component (`src/components/settings/UserProfileEditForm.tsx`):**
        *   Purpose: Form to edit editable user profile information (name, phone).
        *   Props: `initialUserData: UserProfileDto` (to pre-populate form fields).
        *   State: Manage form state using `useState` for editable fields (firstName, lastName, phone - potentially others). Initialize state from `initialUserData` props.
        *   Structure:
            *   Use a `Card` or `Box` to contain the form.
            *   Form elements:
                *   `FormInputField` components for "First Name", "Last Name", "Phone Number".  Pre-populate `value` prop with data from `initialUserData`.
            *   `FormButton` component for "Save Changes" (type="submit").
            *   Placeholder for displaying success/error messages.
            *   Form Structure Example:
                ```jsx
                <Card>
                    <CardContent>
                        <Typography variant="h6" gutterBottom>Edit Profile</Typography>
                        <form onSubmit={() => { /* Placeholder - handle submit */ }}>
                            <Grid container spacing={2}>
                                <Grid item xs={12} sm={6}>
                                    <FormInputField label="First Name" id="firstName" value={/* state.firstName */} onChange={() => { /* update state */ }} />
                                </Grid>
                                <Grid item xs={12} sm={6}>
                                    <FormInputField label="Last Name" id="lastName" value={/* state.lastName */} onChange={() => { /* update state */ }} />
                                </Grid>
                                <Grid item xs={12}>
                                    <FormInputField label="Phone Number" id="phoneNumber" type="tel" value={/* state.phoneNumber */} onChange={() => { /* update state */ }} />
                                </Grid>
                                <Grid item xs={12}>
                                    <FormButton type="submit">Save Changes</FormButton>
                                </Grid>
                            </Grid>
                        </form>
                        {/* Placeholder for success/error message */}
                    </CardContent>
                </Card>
                ```

    *   **`ChangePasswordForm` Component (`src/components/settings/ChangePasswordForm.tsx`):**
        *   Purpose: Form to allow users to change their password.
        *   State: Manage form state using `useState` for `currentPassword`, `newPassword`, `confirmNewPassword`.
        *   Structure:
            *   Use a `Card` or `Box` to contain the form.
            *   Form elements:
                *   `FormInputField` components for "Current Password" (type="password"), "New Password" (type="password"), "Confirm New Password" (type="password").
            *   `FormButton` component for "Change Password" (type="submit").
            *   Placeholder for displaying success/error messages.
            *   Form Structure Example:
                ```jsx
                <Card>
                    <CardContent>
                        <Typography variant="h6" gutterBottom>Change Password</Typography>
                        <form onSubmit={() => { /* Placeholder - handle submit */ }}>
                            <Grid container spacing={2}>
                                <Grid item xs={12}>
                                    <FormInputField label="Current Password" id="currentPassword" type="password" value={/* state.currentPassword */} onChange={() => { /* update state */ }} />
                                </Grid>
                                <Grid item xs={12}>
                                    <FormInputField label="New Password" id="newPassword" type="password" value={/* state.newPassword */} onChange={() => { /* update state */ }} />
                                </Grid>
                                <Grid item xs={12}>
                                    <FormInputField label="Confirm New Password" id="confirmNewPassword" type="password" value={/* state.confirmNewPassword */} onChange={() => { /* update state */ }} />
                                </Grid>
                                <Grid item xs={12}>
                                    <FormButton type="submit">Change Password</FormButton>
                                </Grid>
                            </Grid>
                        </form>
                        {/* Placeholder for success/error message */}
                    </CardContent>
                </Card>
                ```

    *   **`ProfileSettingsPage` Component (`src/pages/settings/profile.tsx`):**
        *   Purpose:  Main page component to display and manage user profile settings.
        *   Structure:
            *   Basic page layout (e.g., using a `Container` or `Box` from MUI).
            *   Render `UserProfileView`, `UserProfileEditForm`, and `ChangePasswordForm` components within the page.
            *   Arrange components vertically or using tabs/accordions for better organization (for now, vertical stacking is fine).
            *   Heading (`Typography` with "Profile Settings").
            *   Placeholder for fetching user profile data (in `useEffect` - will be implemented in Pages file).
            *   Component Composition Example:
                ```jsx
                <Container>
                    <Typography variant="h4" component="h1" gutterBottom>Profile Settings</Typography>
                    <UserProfileView userData={/* ... user profile data from state */} />
                    <UserProfileEditForm initialUserData={/* ... user profile data from state */} />
                    <ChangePasswordForm />
                </Container>
                ```

**File Structure:**

*   `src/components/settings/UserProfileView.tsx`
*   `src/components/settings/UserProfileEditForm.tsx`
*   `src/components/settings/ChangePasswordForm.tsx`
*   (Ensure `src/components/ui/FormInputField.tsx`, `FormButton.tsx`, `FormErrorMessage.tsx` exist from previous steps)


**Instructions for AI Agent (Summary):**

*   Generate React functional components: `UserProfileView`, `UserProfileEditForm`, `ChangePasswordForm`, and `ProfileSettingsPage`.
*   Use MUI Core components for layout and basic styling, reuse `FormInputField`, `FormButton`, `FormErrorMessage`. Use Joy UI minimally if needed.
*   Implement basic structure for each component as described above, including placeholders for data and form handling.
*   Place components in the specified file structure under `src/components/settings/` and `src/pages/settings/`.

**Next Steps:**

*   After generating these components, proceed to create the Next.js page (`/settings/profile.tsx`) and integrate these components within it in `143_UserProfileManagement_Frontend_Pages.md`.