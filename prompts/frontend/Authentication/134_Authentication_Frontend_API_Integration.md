# 134_Authentication_Frontend_API_Integration.md

**Frontend Feature:** Authentication - Frontend API Integration

**Purpose:**

This file instructs the AI agent to integrate the frontend authentication pages (Login, Registration, Forgot Password, Reset Password) with the backend authentication API endpoints. This will involve implementing form submission handling, API calls using the API client, JWT token management, authentication state management, and error handling.

**Instructions for AI Agent:**

1.  **Import API Client Functions:** In each of the form components (`LoginForm`, `RegistrationForm`, `ForgotPasswordForm`, `ResetPasswordForm`) located in `src/components/auth/`, import the relevant API client functions that should have been generated based on `09_Frontend_Setup_API_Client.md` and `04_Backend_Setup_Authentication.md` + `Auth_Backend_API.md`.

    *   **`LoginForm.tsx`**: Import the login API client function (e.g., `authApi.login`).
    *   **`RegistrationForm.tsx`**: Import the register API client function (e.g., `authApi.register`).
    *   **`ForgotPasswordForm.tsx`**: Import the forgot password API client function (e.g., `authApi.forgotPassword`).
    *   **`ResetPasswordForm.tsx`**: Import the reset password API client function (e.g., `authApi.resetPassword`).

2.  **Implement Form Submission Handlers:**  Modify the `onSubmit` handlers in each form component to:

    *   **Prevent default form submission:**  `event.preventDefault()`.
    *   **Get form data:**  Retrieve the form data from the component's state (email, password, name, etc.).
    *   **Call the relevant API client function:**
        *   **`LoginForm`**: Call `authApi.login({ email, password })`.
        *   **`RegistrationForm`**: Call `authApi.register({ firstName, lastName, email, password, role })` (*adjust parameters based on your registration form fields and backend API definition*).
        *   **`ForgotPasswordForm`**: Call `authApi.forgotPassword({ email })`.
        *   **`ResetPasswordForm`**: Call `authApi.resetPassword({ newPassword, confirmPassword, resetToken })` (*ensure you are getting the `resetToken` from the page's router parameters and passing it to the component as a prop*).

3.  **Handle API Responses and Errors:**  Within each form's `onSubmit` handler, after calling the API client function:

    *   **Handle Success Responses (2xx status codes):**
        *   **Login Success:**
            *   Store the JWT token received in the response (e.g., in `localStorage` or `cookies` - for now, use `localStorage` for simplicity, but consider HttpOnly cookies for production). *Discuss security implications of localStorage vs. cookies and suggest secure cookie approach for production later.*
            *   Redirect the user to the main application dashboard (`/dashboard` - we'll create this dashboard page later. For now, redirect to `/`).
            *   *(Potentially) Store user information in application state (e.g., using Context API or Zustand/Recoil) for access throughout the application.*

        *   **Registration Success:**
            *   Display a success message to the user (e.g., "Registration successful! You can now log in.").
            *   Redirect the user to the login page (`/auth/login`).
            *   *(Alternatively, you could auto-login the user after successful registration - consider UX implications).*

        *   **Forgot Password Success:**
            *   Display a success message (e.g., "If an account with that email exists, a password reset link has been sent.").
            *   Potentially redirect back to the login page after a short delay.

        *   **Reset Password Success:**
            *   Display a success message (e.g., "Password reset successful! You can now log in with your new password.").
            *   Redirect the user to the login page (`/auth/login`).

    *   **Handle Error Responses (4xx, 5xx status codes):**
        *   **For all API calls:**
            *   Catch any errors that occur during the API call (using `.catch()` or `try...catch` with async/await).
            *   Extract the error message from the API response (if available in the response body - backend should return meaningful error messages).
            *   Set the error message in the component's state.
            *   Display the error message using the `FormErrorMessage` component below the relevant form field or at the top of the form.  *Ensure error messages are user-friendly and helpful.*

4.  **Authentication State Management (Basic - using `localStorage` and Context):**
    *   **(Initial Basic Implementation - can be improved with more robust state management later):**
    *   Create an Authentication Context (`src/contexts/AuthContext.tsx`) to manage the user's authentication state (logged in status, user information, JWT token).
    *   In `AuthContext.tsx`:
        *   Create a React Context using `React.createContext`.
        *   Create an `AuthProvider` component that wraps the application in `_app.tsx`.
        *   Inside `AuthProvider`, manage authentication state using `useState` (initially, check for JWT token in `localStorage` on component mount to determine initial logged-in state).
        *   Provide functions in the context to:
            *   `login(token)`: Stores the JWT token in `localStorage` and updates the authentication state to 'logged in'.
            *   `logout()`: Removes the JWT token from `localStorage` and updates the authentication state to 'logged out'.
            *   `isAuthenticated()`: Returns the current logged-in status based on the presence of a JWT token in `localStorage`.
            *   Potentially `getUserInfo()`:  To retrieve and store user information (decode from JWT or fetch from backend - *for now, just focus on token storage and logged-in status*).
        *   Make the `isAuthenticated()` function and potentially `login()`/`logout()` functions accessible to components via the context.

    *   **Wrap `_app.tsx` with `AuthProvider`:**  In `_app.tsx`, wrap the `<MyApp>` component with the `<AuthProvider>` to make the authentication context available throughout the application.

5.  **Implement Authentication Guard/Protected Routes (Basic - Redirect on Login):**
    *   **(Basic Route Protection - can be enhanced with proper Guards later):**
    *   For now, in the `pages/index.tsx` (or the main dashboard page if you have created one), implement a basic check in `useEffect` to see if the user is authenticated using `AuthContext.isAuthenticated()`.
    *   If not authenticated, redirect the user to the login page (`/auth/login`) using `Next/Router`.
    *   *More robust route guarding will be implemented later using NestJS-like Guards on the frontend, but this basic redirect is sufficient for initial testing.*

**File Modifications:**

*   `src/components/auth/LoginForm.tsx` (Modify `onSubmit` handler, import API client, handle API response, error, redirect)
*   `src/components/auth/RegistrationForm.tsx` (Modify `onSubmit` handler, import API client, handle API response, error, redirect)
*   `src/components/auth/ForgotPasswordForm.tsx` (Modify `onSubmit` handler, import API client, handle API response, error)
*   `src/components/auth/ResetPasswordForm.tsx` (Modify `onSubmit` handler, import API client, handle API response, error, handle resetToken prop)
*   `src/contexts/AuthContext.tsx` (Create new file - Authentication Context provider and logic)
*   `pages/_app.tsx` (Wrap `<MyApp>` with `<AuthProvider>`)
*   `pages/index.tsx` (or dashboard page - Implement basic authentication check and redirect)


**Instructions for AI Agent (Summary):**

*   Import relevant API client functions into form components.
*   Implement form submission handlers to call API client functions for login, registration, forgot password, reset password.
*   Handle success and error responses from the API calls, display error messages, redirect on success.
*   Implement basic authentication state management using React Context and `localStorage`.
*   Create `AuthContext`, `AuthProvider`, `login`, `logout`, `isAuthenticated` functions.
*   Wrap `_app.tsx` with `AuthProvider`.
*   Implement basic route protection in `pages/index.tsx` (or dashboard page) to redirect to login if not authenticated.

**Next Steps:**

*   After implementing API integration and authentication logic, we will have a functional authentication flow in the frontend!
*   The next step would be to test the authentication functionality thoroughly: login, logout, registration, forgot password, reset password, error handling.