# 133_Authentication_Frontend_Pages.md

**Frontend Feature:** Authentication - Next.js Pages

**Purpose:**

This file defines the Next.js pages for the Authentication feature (Login, Registration, Forgot Password, Reset Password). It instructs the AI agent to create these pages in the `pages/auth/` directory, configure routing, and integrate the React components defined in `132_Authentication_Frontend_Components.md` to build the UI for each page.

**Instructions for AI Agent:**

1.  **Create Next.js Pages:** Generate the following Next.js pages under the `pages/auth/` directory. Create the `auth` directory first if it doesn't exist within `pages/`.

2.  **Page File Structure:** Create the following files within `pages/auth/`:
    *   `pages/auth/login.tsx` (Login Page)
    *   `pages/auth/register.tsx` (Registration Page)
    *   `pages/auth/forgot-password.tsx` (Forgot Password Page)
    *   `pages/auth/reset-password/[resetToken].tsx` (Reset Password Page - dynamic route parameter for reset token)

3.  **Implement Page Content and Routing:** For each page:
    *   **Import necessary React components:** Import the relevant form components (`LoginForm`, `RegistrationForm`, `ForgotPasswordForm`, `ResetPasswordForm`) and the `AuthLayout` component from `src/components/auth/`.
    *   **Wrap content in `AuthLayout`:**  Use the `AuthLayout` component to structure the page layout and wrap the main form content within it.
    *   **Render Form Components:** Render the appropriate form component (`LoginForm` in `login.tsx`, `RegistrationForm` in `register.tsx`, etc.) within each page.
    *   **Basic Page Structure:**  Set up the basic page structure with a functional component, import statements, and rendering of the form component within `AuthLayout`.
    *   **Routing (Next.js `Link` component):**  Ensure that the Login page has links to the "Forgot Password" and potentially "Register" pages (using `<Link href="/auth/forgot-password">` and `<Link href="/auth/register">`). The Forgot Password page should have a link back to the Login page.
    *   **Dynamic Route for Reset Password:** For `reset-password/[resetToken].tsx`, ensure it's set up as a dynamic route to capture the `resetToken` from the URL.  *For now, just access the `resetToken` using `useRouter()` from `next/router` and log it to the console within the page component - we'll handle token validation and API interaction later.*

4.  **Basic Functionality (Placeholders - API integration in future steps):**
    *   For each page, within the main page component function (e.g., `LoginPage` in `login.tsx`):
        *   Pass any necessary props to the form components (though initially, the form components are self-contained and manage their own state).
        *   *No API calls yet - form submission handling in the components is currently just logging data to the console as defined in `132_Authentication_Frontend_Components.md`.*

5.  **Page Component Names:** Use descriptive component names for each page:
    *   `LoginPage` for `login.tsx`
    *   `RegistrationPage` for `register.tsx`
    *   `ForgotPasswordPage` for `forgot-password.tsx`
    *   `ResetPasswordPage` for `reset-password/[resetToken].tsx`

**File Structure:**

*   `pages/auth/login.tsx`
*   `pages/auth/register.tsx`
*   `pages/auth/forgot-password.tsx`
*   `pages/auth/reset-password/[resetToken].tsx`
*   (Ensure `src/components/auth/` and `src/components/ui/` components from `132_Authentication_Frontend_Components.md` are also in place)

**Instructions for AI Agent (Summary):**

*   Create Next.js pages in `pages/auth/` directory as specified above.
*   Implement routing between pages using `<Link>` component.
*   Use dynamic routing for `reset-password/[resetToken].tsx` and access the `resetToken` parameter (for now, just log to console).
*   Import and render the form components (`LoginForm`, `RegistrationForm`, etc.) and `AuthLayout` component within each page to construct the UI.
*   Set up basic page structure and component composition, placeholder functionality (form submit logs to console).

**Next Steps:**

*   After generating these pages, we will have the basic frontend UI for Authentication in place.
*   The next steps will be to:
    *   Implement form validation (client-side).
    *   Connect the frontend forms to the backend authentication API endpoints to implement actual login, registration, forgot password, and reset password functionality.