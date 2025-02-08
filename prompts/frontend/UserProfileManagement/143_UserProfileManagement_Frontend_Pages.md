# 143_UserProfileManagement_Frontend_Pages.md

**Frontend Feature:** User Profile Management - Next.js Page

**Purpose:**

This file defines the Next.js page for User Profile Management, located at `/settings/profile`. It instructs the AI agent to create this page, configure the route, fetch user profile data, and integrate the React components defined in `142_UserProfileManagement_Frontend_Components.md` to build the User Profile Settings page.

**Instructions for AI Agent:**

1.  **Create Next.js Page:** Generate the Next.js page file `profile.tsx` within the `pages/settings/` directory. Create the `settings` directory first if it doesn't exist within `pages/`.

2.  **Page File Content (`pages/settings/profile.tsx`):**

    *   **Import necessary React components:**
        *   Import `UserProfileView`, `UserProfileEditForm`, `ChangePasswordForm` from `src/components/settings/`.
        *   Import `Container`, `Typography` from `@mui/material` (or relevant MUI layout components).
        *   Import `useEffect` and `useState` from React.
        *   Import `useRouter` from `next/router`.
        *   Import `useContext` and `AuthContext` from `src/contexts/AuthContext`.

    *   **Create `ProfileSettingsPage` Functional Component:**
        *   Define a functional component named `ProfileSettingsPage`.
        *   Implement basic page structure using JSX.
        *   Use `Container` from MUI to wrap the page content for layout.
        *   Add a `Typography` component with variant `h4` or `h5` for the page title "Profile Settings".

    *   **Fetch User Profile Data (Placeholder - No API call yet, mock data):**
        *   Use `useState` to manage user profile data within the page component: `const [userData, setUserData] = useState(null);`.
        *   Use `useEffect` hook to simulate fetching user profile data when the component mounts:
            ```javascript
            useEffect(() => {
                // Simulate fetching user profile data (replace with actual API call in API Integration step)
                const mockProfileData = {
                    firstName: "John",
                    lastName: "Doe",
                    email: "[email address removed]",
                    phone: "123-456-7890",
                    role: "Veterinarian"
                };
                setUserData(mockProfileData);
                console.log("Simulated profile data fetched:", mockProfileData); // Log for verification
            }, []);
            ```
        *   *In this step, we are using mock data to ensure the UI structure is correct. We will connect to the actual backend API in the API Integration step.*
        *   Add a loading state and display a loading indicator while data is being "fetched" (even with mock data, simulate a short delay using `setTimeout` within `useEffect` if desired for better visual feedback).

    *   **Render Components:**
        *   Render the `UserProfileView`, `UserProfileEditForm`, and `ChangePasswordForm` components within the `ProfileSettingsPage` component.
        *   Pass the `userData` state as props to the `UserProfileView` and `UserProfileEditForm` components: `<UserProfileView userData={userData} />` and `<UserProfileEditForm initialUserData={userData} />`.
        *   Render `ChangePasswordForm` without props for now: `<ChangePasswordForm />`.

    *   **Implement Authentication Guard (Basic Redirect if not authenticated):**
        *   Use `useContext(AuthContext)` to access the `isAuthenticated` function from the `AuthContext`.
        *   Use `useEffect` and `useRouter` to implement a basic authentication guard:
            ```javascript
            const router = useRouter();
            const { isAuthenticated } = useContext(AuthContext);

            useEffect(() => {
                if (!isAuthenticated()) {
                    router.push('/auth/login'); // Redirect to login page if not authenticated
                }
            }, [isAuthenticated, router]);

            if (!isAuthenticated()) {
                return null; // Or return a loading/redirecting indicator, or a message like "Redirecting to login..."
            }
            ```
        *   *This is a basic client-side redirect. More robust route guarding might be needed later, but this is sufficient for initial functionality.*

    *   **Basic Page Structure Example (`pages/settings/profile.tsx`):**

        ```typescript jsx
        import React, { useEffect, useState, useContext } from 'react';
        import { Container, Typography } from '@mui/material';
        import { UserProfileView } from '@/components/settings/UserProfileView'; // Adjust import paths if necessary
        import { UserProfileEditForm } from '@/components/settings/UserProfileEditForm'; // Adjust import paths
        import { ChangePasswordForm } from '@/components/settings/ChangePasswordForm'; // Adjust import paths
        import { useRouter } from 'next/router';
        import { AuthContext } from '@/contexts/AuthContext'; // Adjust import paths

        const ProfileSettingsPage: React.FC = () => {
            const [userData, setUserData] = useState(null);
            const router = useRouter();
            const { isAuthenticated } = useContext(AuthContext);

            useEffect(() => {
                if (!isAuthenticated()) {
                    router.push('/auth/login');
                    return; // Important to return to prevent further execution in useEffect
                }

                // Simulate fetching profile data
                const mockProfileData = {
                    firstName: "John",
                    lastName: "Doe",
                    email: "[email address removed]",
                    phone: "123-456-7890",
                    role: "Veterinarian"
                };
                setTimeout(() => { // Simulate network delay
                    setUserData(mockProfileData);
                }, 500); // 500ms delay
            }, [isAuthenticated, router]);

            if (!isAuthenticated()) {
                return null; // Or indicate redirecting
            }

            if (!userData) {
                return <div>Loading profile...</div>; // Or a loading spinner
            }

            return (
                <Container>
                    <Typography variant="h4" component="h1" gutterBottom>Profile Settings</Typography>
                    <UserProfileView userData={userData} />
                    <UserProfileEditForm initialUserData={userData} />
                    <ChangePasswordForm />
                </Container>
            );
        };

        export default ProfileSettingsPage;
        ```

3.  **Create `settings` directory under `pages/` if it doesn't exist.**

**File Structure:**

*   `pages/settings/profile.tsx`
*   (Ensure `src/components/settings/UserProfileView.tsx`, `UserProfileEditForm.tsx`, `ChangePasswordForm.tsx` and `src/contexts/AuthContext.tsx` exist from previous steps)

**Instructions for AI Agent (Summary):**

*   Create `profile.tsx` page in `pages/settings/`.
*   Implement basic page structure with `ProfileSettingsPage` component.
*   Fetch mock user profile data in `useEffect` and manage with `useState`.
*   Render `UserProfileView`, `UserProfileEditForm`, `ChangePasswordForm` components, passing mock data as props.
*   Implement basic authentication guard to redirect to login page if not authenticated.

**Next Steps:**

*   After generating this page, we will have a visible User Profile Settings page in our frontend, with mock data and basic structure!
*   The next step will be to implement API integration in the form components and the page to connect to the backend User Profile Management API endpoints and fetch real user data.