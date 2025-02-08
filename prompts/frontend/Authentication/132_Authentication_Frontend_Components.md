# 132_Authentication_Frontend_Components.md

**Frontend Feature:** Authentication - React Components

**Purpose:**

This file defines the React components for the Authentication feature (Login, Registration, Forgot Password, Reset Password). It instructs the AI agent to generate these components using MUI Core, Joy UI, and Shadcn UI, focusing on reusability, styling, and basic form structure.

**Instructions for AI Agent:**

1.  **Create React Functional Components:** Generate the following React functional components using TypeScript and JSX.

2.  **Utilize UI Libraries:**
    *   **Joy UI Templates as Layouts:**  Use the structure and styling from the Joy UI "Sign In Side" template as a foundation for the `AuthLayout` component and for the overall layout of `LoginForm`, `RegistrationForm`, and `ForgotPasswordForm`.  Adapt the template's structure to fit our specific form fields and branding.
    *   **MUI Core Components for Form Elements & Basic Styling:** Use MUI Core components like `TextField` for input fields, `Button` for buttons, `Typography` for text, `Box` and `Grid` for layout within the forms.  Utilize MUI's styling and theming capabilities for consistent look and feel.
    *   **Shadcn UI Components for Enhanced Styling & Specific Elements (Optional - if needed):** If specific styling needs arise or if Shadcn UI offers components that better suit a particular form element (e.g., more customizable input styles), consider using Shadcn UI components selectively within the forms.

3.  **Implement Basic Form Structure:**  Each form component should:
    *   Manage its own form state using `useState` hook for input fields (email, password, name, etc.).
    *   Include basic form structure using JSX elements (e.g., `<form>`, `<label>`, `<input>`, `<button>`).
    *   Include placeholders for form submission handling (e.g., `onSubmit` handler that currently logs form data to console - API integration will be in Pages file).
    *   Include placeholders for displaying error messages (using `FormErrorMessage` component - defined below).

4.  **Reusable Components:** Create the following reusable components:

    *   **`AuthLayout` Component (`src/components/auth/AuthLayout.tsx`):**
        *   Purpose:  Provides a consistent layout structure for all authentication pages (Login, Register, Forgot Password, Reset Password).
        *   Based on: Adapt the structure from Joy UI "Sign In Side" template. Include a side image/illustration and the main content area for the form.
        *   Content:  Should accept `children` prop to render the specific form content within the layout.
        *   Styling: Use Joy UI and MUI styling to match the "Sign In Side" template visually, but allow for customization.

    *   **`FormInputField` Component (`src/components/ui/FormInputField.tsx`):**
        *   Purpose: Reusable styled input field component for forms across the application.
        *   Based on: MUI `TextField` component.
        *   Props:  `id`, `label`, `type` (text, password, email, etc.), `value`, `onChange`, `error` (boolean), `helperText` (error message), and any other props that MUI `TextField` accepts.
        *   Styling: Apply consistent styling using MUI theme, consider making it customizable with additional style props.

    *   **`FormButton` Component (`src/components/ui/FormButton.tsx`):**
        *   Purpose: Reusable styled button component for forms.
        *   Based on: MUI `Button` component.
        *   Props: `children` (button text), `onClick`, `variant` (MUI button variants - contained, outlined, etc.), `color` (MUI button colors), `disabled`, `type` (submit, button), and any other relevant MUI `Button` props.
        *   Styling:  Apply consistent styling using MUI theme, customizable with style props.

    *   **`FormErrorMessage` Component (`src/components/ui/FormErrorMessage.tsx`):**
        *   Purpose: Reusable component to display form validation or error messages.
        *   Based on: MUI `FormHelperText` or simple `<Typography>` with error styling.
        *   Props: `message` (string - error message to display).
        *   Styling:  Use error styling from MUI theme (e.g., error color).

5.  **Form Components (Authentication Specific):**

    *   **`LoginForm` Component (`src/components/auth/LoginForm.tsx`):**
        *   Purpose:  Login form component for the Login Page.
        *   Structure:
            *   Wrap in `<AuthLayout>` component.
            *   Form with Email and Password `FormInputField` components.
            *   "Login" `FormButton` component (type="submit").
            *   Link to "Forgot Password" page (using `Next/Link` and MUI `Link` component).
            *   *(Optional)* Link to "Register" page.
        *   Form Logic:  Manage email and password input state, `onSubmit` handler to log form data.

    *   **`RegistrationForm` Component (`src/components/auth/RegistrationForm.tsx`):**
        *   Purpose: Registration form component for the Registration Page.
        *   Structure:
            *   Wrap in `<AuthLayout>` component (or decide on a different layout if "Sign In Side" template is less suitable for registration).
            *   Form with Name (First, Last), Email, Password, Confirm Password, and Role selection (initially just a text input for Role, can be dropdown later) `FormInputField` components.
            *   "Register" `FormButton` component (type="submit").
        *   Form Logic: Manage input states, `onSubmit` handler to log form data.

    *   **`ForgotPasswordForm` Component (`src/components/auth/ForgotPasswordForm.tsx`):**
        *   Purpose: Forgot Password form component for the Forgot Password Page.
        *   Structure:
            *   Wrap in `<AuthLayout>` component.
            *   Form with Email `FormInputField` component.
            *   "Send Reset Link" `FormButton` component (type="submit").
            *   Link back to "Login" page.
        *   Form Logic: Manage email input state, `onSubmit` handler to log form data.

    *   **`ResetPasswordForm` Component (`src/components/auth/ResetPasswordForm.tsx`):**
        *   Purpose: Reset Password form component for the Reset Password Page.
        *   Structure:
            *   (Layout - decide if `AuthLayout` is suitable or simpler layout)
            *   Form with New Password and Confirm New Password `FormInputField` components.
            *   "Reset Password" `FormButton` component (type="submit").
        *   Form Logic: Manage new password and confirm password input states, `onSubmit` handler to log form data.  Need to handle reset token (passed via URL parameter - will be handled in Pages file and passed down to component as prop).


**File Structure:**

*   `src/components/auth/AuthLayout.tsx`
*   `src/components/auth/LoginForm.tsx`
*   `src/components/auth/RegistrationForm.tsx`
*   `src/components/auth/ForgotPasswordForm.tsx`
*   `src/components/auth/ResetPasswordForm.tsx`
*   `src/components/ui/FormInputField.tsx`
*   `src/components/ui/FormButton.tsx`
*   `src/components/ui/FormErrorMessage.tsx`


**Instructions for AI Agent (Summary):**

*   Generate React functional components as described above.
*   Use Joy UI "Sign In Side" template as layout foundation, MUI Core for form elements and basic styling, and Shadcn UI selectively if needed.
*   Implement basic form structure with input fields, buttons, and error message placeholders.
*   Create reusable `AuthLayout`, `FormInputField`, `FormButton`, `FormErrorMessage` components.
*   Create authentication-specific form components: `LoginForm`, `RegistrationForm`, `ForgotPasswordForm`, `ResetPasswordForm`.
*   Place components in the specified file structure under `src/components/`.

**Next Steps:**

*   After generating these components, proceed to create the Next.js pages for Authentication in `133_Authentication_Frontend_Pages.md`.