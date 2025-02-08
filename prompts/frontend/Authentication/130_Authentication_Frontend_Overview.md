# 130_Authentication_Frontend_Overview.md

**Frontend Feature:** Authentication (Login, Registration, Forgot Password)

**Purpose:**

This feature implements the user authentication flow for the Veterinary PMS frontend. It will allow users (clinic staff) to securely log in to the application, register new accounts (initially for administrators and staff onboarding), and recover their passwords if forgotten.  Secure and functional authentication is the gateway to the entire application.

**User Stories:**

*   As a **Clinic Staff Member**, I want to be able to **log in to the Veterinary PMS** using my credentials (email and password) so I can access the system and perform my tasks.
*   As a **Clinic Administrator**, I want to be able to **create new user accounts** for new staff members so they can access the system. (Registration flow will initially be for staff accounts, client registration might be a future enhancement).
*   As a **User**, I want to be able to **recover my password** if I forget it, so I can regain access to my account.
*   As a **User**, I want to see **clear error messages** if I enter incorrect login credentials or during registration, so I understand what went wrong.
*   As a **User**, I want the **authentication process to be secure** to protect my account and clinic data.

**Key Pages:**

*   **Login Page (`/auth/login`):**
    *   Email/Username input field.
    *   Password input field.
    *   "Login" button.
    *   Link to "Forgot Password" page.
    *   Potentially a link to "Register" page (for initial staff onboarding).
    *   Form validation (client-side and potentially server-side).
    *   Error message display for invalid credentials.
    *   Utilize "Sign In Side" template from Joy UI as a starting point.

*   **Registration Page (`/auth/register`):**
    *   (Initially for Clinic Administrators to create staff accounts)
    *   Name input fields (First Name, Last Name).
    *   Email input field (used as username).
    *   Password input field.
    *   Confirm Password input field.
    *   Role selection (Dropdown/Radio - initially limited roles like 'Receptionist', 'Veterinarian', 'Technician' - based on RBAC). *Consider if role selection is on registration page or separate admin user creation page.*
    *   "Register" button.
    *   Form validation (client-side and server-side).
    *   Error message display for registration failures (e.g., email already exists, password mismatch).
    *   Potentially adapt "Sign In Side" template from Joy UI or use a simpler form layout.

*   **Forgot Password Page (`/auth/forgot-password`):**
    *   Email input field (to identify account for password reset).
    *   "Send Reset Link" button.
    *   Confirmation message after sending reset link (e.g., "If an account with that email exists, a password reset link has been sent."). *Avoid revealing if email exists or not for security.*
    *   Link back to "Login" page.

*   **Reset Password Page (`/auth/reset-password/[resetToken]`):**
    *   Accessed via a link in the password reset email.
    *   New Password input field.
    *   Confirm New Password input field.
    *   "Reset Password" button.
    *   Form validation (client-side and server-side - token validation, password strength, password match).
    *   Success message upon password reset.
    *   Link to "Login" page.
    *   Handle expired or invalid reset tokens.

**Key Components (React Components):**

*   **LoginForm Component:**  Reusable form component for the Login Page.
*   **RegistrationForm Component:** Reusable form component for the Registration Page.
*   **ForgotPasswordForm Component:** Reusable form component for the Forgot Password Page.
*   **ResetPasswordForm Component:** Reusable form component for the Reset Password Page.
*   **AuthLayout Component:** (Potentially) A layout component to wrap authentication pages (login, register, forgot password) to provide consistent styling and structure (e.g., using the "Sign In Side" template structure).
*   **FormInputField Component:** (Reusable)  Styled input field component (using MUI or Shadcn UI).
*   **FormButton Component:** (Reusable) Styled button component (using MUI or Shadcn UI).
*   **FormErrorMessage Component:** (Reusable) Component to display form validation errors.

**Data Flow and API Interactions:**

*   **Login:**
    *   LoginForm submits user credentials (email, password) to the backend `/auth/login` API endpoint (already defined in `04_Backend_Setup_Authentication.md` and `Auth_Backend_API.md`).
    *   Backend API authenticates user and returns JWT token upon successful login.
    *   Frontend stores JWT token (e.g., in local storage or cookies - consider security implications and best practices).
    *   Frontend redirects to the main application dashboard upon successful login.

*   **Registration:**
    *   RegistrationForm submits user data (name, email, password, role) to the backend `/auth/register` API endpoint (already defined in `04_Backend_Setup_Authentication.md` and `Auth_Backend_API.md`).
    *   Backend API creates a new user account and returns success/failure.
    *   Frontend displays success or error message.  Potentially redirects to login page after successful registration, or auto-logs in the user (depending on desired flow).

*   **Forgot Password:**
    *   ForgotPasswordForm submits email to backend `/auth/forgot-password` API endpoint (already defined in `04_Backend_Setup_Authentication.md` and `Auth_Backend_API.md`).
    *   Backend API generates password reset token, sends reset email to user.
    *   Frontend displays confirmation message.

*   **Reset Password:**
    *   ResetPasswordForm submits new password and reset token to backend `/auth/reset-password/:token` API endpoint (already defined in `04_Backend_Setup_Authentication.md` and `Auth_Backend_API.md`).
    *   Backend API validates reset token, updates password, and returns success/failure.
    *   Frontend displays success or error message.  Redirects to login page upon successful password reset.

**Instructions for AI Agent:**

*   Based on this overview and the UI library setup (Joy UI, MUI, Shadcn UI), proceed to create the React components for the Authentication feature in `132_Authentication_Frontend_Components.md`.
*   Focus on creating the form components (LoginForm, RegistrationForm, ForgotPasswordForm, ResetPasswordForm), reusable input and button components, and potentially an AuthLayout component.
*   Utilize Joy UI "Sign In Side" template structure as a starting point for the Login, Register, and Forgot Password pages/layouts.  Incorporate MUI Core and Shadcn UI components for form elements and styling as needed.
*   Ensure components are designed for reusability, maintainability, and good user experience.
*   After defining the components, proceed to create the Next.js pages in `133_Authentication_Frontend_Pages.md`, using these components to build the Login, Registration, Forgot Password, and Reset Password pages and configure routing.