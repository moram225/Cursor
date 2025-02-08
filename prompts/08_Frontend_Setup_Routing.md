# 08_Frontend_Setup_Routing.md

**Purpose:**
This file guides the AI Agent to set up the basic frontend routing structure for the Veterinary PMS Next.js application using the App Router. This involves creating directories and initial `page.tsx` files within the `src/app` directory to define the main routes of the application.

**Instructions:**

1.  **Define Main Routes based on Core Features:**
    *   Based on the core features of the Veterinary PMS, we need to establish the following main routes in the frontend application. Using Next.js App Router, this is primarily done by creating directories within the `src/app` directory.

    *   Create the following directories directly under the `src/app` directory. Each directory will represent a main route.  If the directory already exists, ensure it is present:

        *   `dashboard` (for the main dashboard/home page)
        *   `patients`
        *   `clients`
        *   `billing`
        *   `inventory`
        *   `reporting`
        *   `communication`
        *   `superadmin`
        *   `auth` (for authentication related pages like login, register)

2.  **Create initial `page.tsx` files for each main route:**
    *   Inside each of the directories created in step 1 (e.g., `src/app/dashboard`, `src/app/patients`, etc.), create a `page.tsx` file.  This file will be the default page rendered when navigating to that route (e.g., `/dashboard`, `/patients`).
    *   In each `page.tsx` file, add a basic functional component that renders a heading indicating the route name.  This is just for placeholder and verification.

    *   Example `src/app/dashboard/page.tsx`:

    ```tsx  typescript
    // src/app/dashboard/page.tsx
    export default function DashboardPage() {
      return (
        <div>
          <h1>Dashboard</h1>
          <p>Welcome to the main dashboard page.</p>
        </div>
      );
    }
    ```

    *   Repeat this for all directories created in step 1, adjusting the heading and description in each `page.tsx` to reflect the route name (e.g., in `src/app/patients/page.tsx`, the heading should be "Patients", etc.).

3.  **Create nested routes under `auth` (optional but recommended):**
    *   Within the `src/app/auth` directory, create subdirectories for `login` and `register` to create nested routes like `/auth/login` and `/auth/register`.
    *   Inside `src/app/auth/login` and `src/app/auth/register`, create `page.tsx` files similar to step 2, with headings like "Login" and "Register" respectively.

    *   Example `src/app/auth/login/page.tsx`:

    ```tsx  typescript
    // src/app/auth/login/page.tsx
    export default function LoginPage() {
      return (
        <div>
          <h1>Login</h1>
          <p>Login page for Veterinary PMS.</p>
        </div>
      );
    }
    ```

4.  **Root Layout Verification (Optional):**
    *   **(Optional):**  You can check the `src/app/layout.tsx` file (which should have been created by `create-next-app`). This file defines the root layout for your application.  You can add a basic navigation structure here later to link to these main routes if you want to visually verify the routing in the browser, but this is not required for this setup file. We will focus on navigation setup in later files.  For now, just ensure `layout.tsx` exists in `src/app`.


**Context:**

*   We are setting up the frontend routing structure using Next.js App Router.
*   App Router uses file-system based routing, where directories in `src/app` define URL paths.
*   This setup establishes the main navigation structure for the Veterinary PMS frontend, aligning with the core features.
*   We are creating placeholder `page.tsx` files for each route, which will be replaced with actual page content in later steps.

**Rules and Constraints:**

*   Use Next.js App Router for routing.
*   Create directories under `src/app` to define main routes.
*   Create `page.tsx` files within each route directory to define the page component for that route.
*   Create routes for `dashboard`, `patients`, `clients`, `billing`, `inventory`, `reporting`, `communication`, `superadmin`, and nested `auth/login`, `auth/register` routes.
*   Keep the `page.tsx` files minimal with placeholder headings and descriptions for verification.

**Details:**

*   Routing is based on directories under `src/app`.
*   Default page component for each route is `page.tsx` inside the route directory.
*   Nested routes are created by nesting directories (e.g., `auth/login`).

**Verification:**

*   After executing these instructions, the AI Agent should confirm:
    *   Creation of all specified directories under `src/app`: `dashboard`, `patients`, `clients`, `billing`, `inventory`, `reporting`, `communication`, `superadmin`, `auth`.
    *   Creation of `page.tsx` files within each of these directories.
    *   Creation of `login` and `register` subdirectories under `src/app/auth`.
    *   Creation of `page.tsx` files within `src/app/auth/login` and `src/app/auth/register`.
    *   **(Optional):** If you choose to verify in browser, navigating to `/dashboard`, `/patients`, `/billing`, `/auth/login`, `/auth/register`, etc., should render the corresponding placeholder pages with the correct headings.

**Next Steps (for AI Agent):**

1.  Execute the steps outlined in the "Instructions" section, creating the directories and `page.tsx` files.
2.  Verify each step as described in the "Verification" section.
3.  Provide a summary of actions taken and confirmation of successful frontend routing setup.
4.  Once confirmed, proceed to the next file: `09_Frontend_Setup_API_Client.md`.