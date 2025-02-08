# 06_Frontend_Setup_NextJS.md

**Purpose:**
This file guides the AI Agent to set up the basic Next.js frontend project for the Veterinary PMS. This includes initializing the project, setting up the project structure, and installing essential frontend dependencies.

**Instructions:**

1.  **Create a new Next.js project:**
    *   Use `create-next-app` to create a new Next.js project named `veterinary-pms-frontend`.
    *   Use `npm` as the package manager.
    *   Enable TypeScript during project setup.
    *   Enable Tailwind CSS during project setup.
    *   Enable `src` directory during project setup.
    *   Enable App Router during project setup.
    *   Answer "No" to the question about customizing the default import alias.

    ```bash
    npx create-next-app@latest veterinary-pms-frontend
    ```

    *(Follow the prompts from `create-next-app` to enable TypeScript, Tailwind CSS, `src` directory and App Router, and choose no for customizing import alias).*

    ```bash
    cd veterinary-pms-frontend
    ```

2.  **Install core frontend dependencies:**
    *   Ensure the following core frontend modules are installed. If not already present in `package.json` and `node_modules`, install them using npm:
        *   `react`
        *   `react-dom`
        *   `next`

    ```bash
    npm install react react-dom next
    ```

3.  **Basic Project Structure Setup within `src` directory:**
    *   **(App Router Structure is default with `create-next-app`):**  Verify the following structure exists within the `src/app` directory (this is the default App Router structure in Next.js):
        *   `layout.tsx`:  Root layout for the application.
        *   `page.tsx`:  Default home page of the application.
    *   Create the following folders within the `src` directory to organize the frontend features logically (alongside the `app` directory):
        *   `components`: For reusable React components.
        *   `styles`: For global styles and CSS modules (Tailwind CSS is already configured).
        *   `utils`: For utility functions and helpers.
        *   `services`: For API client services (for interacting with the backend).
        *   `contexts`: For React Contexts if needed for state management.
        *   `assets`: For static assets like images, icons, etc.
        *   `auth`: For authentication related frontend components and logic (e.g., login form, signup form).
        *   `patients`: For patient management frontend features.
        *   `clients`: For client management frontend features.
        *   `billing`: For billing and financial frontend features.
        *   `inventory`: For inventory and supply chain frontend features.
        *   `reporting`: For reporting and analytics frontend features.
        *   `communication`: For communication tools frontend features.
        *   `superadmin`: For SuperAdmin interface frontend features.


4.  **Initial Configuration (environment variables for frontend - optional for now):**
    *   For now, we might not need specific frontend environment variables at this stage.  If needed later (e.g., for API base URL), we'll add instructions to create a `.env.local` file in the root of `veterinary-pms-frontend` and configure environment variables.  For now, we can skip explicit environment variable setup for the frontend, and configure API base URL directly in the API client service (which will be created later).

**Context:**

*   We are setting up the frontend for the Veterinary PMS using Next.js, a React framework for building performant web applications.
*   This is the first step in building the frontend infrastructure.
*   Subsequent files will build upon this foundation to add UI libraries, routing, API integration, and core features.
*   We are using the App Router feature of Next.js.

**Rules and Constraints:**

*   Use `create-next-app` for project creation.
*   Use `npm` as the package manager.
*   Use TypeScript for the frontend.
*   Enable Tailwind CSS during project setup.
*   Enable `src` directory and App Router during project setup.
*   Adhere to Next.js project structure conventions and App Router structure.
*   Create the specified folder structure within the `src` directory.
*   Keep the initial setup minimal and focused on project initialization and core dependencies.

**Details:**

*   Project name: `veterinary-pms-frontend`
*   Package manager: `npm`
*   Language: `TypeScript`
*   Tailwind CSS: Enabled
*   App Router: Enabled
*   `src` directory: Enabled

**Verification:**

*   After executing these instructions, the AI Agent should confirm:
    *   Successful creation of the Next.js project named `veterinary-pms-frontend`.
    *   Installation of core React, ReactDOM, and Next.js dependencies.
    *   Verification of the default App Router structure within `src/app` (`layout.tsx`, `page.tsx`).
    *   Creation of the specified folder structure within the `src` directory (alongside `app`).
    *   Tailwind CSS configuration is set up (verify `tailwind.config.js` and `postcss.config.js` in the project root - or within `src` if Next.js project structure changed).

**Next Steps (for AI Agent):**

1.  Execute the commands and steps outlined in the "Instructions" section.
2.  Verify each step as described in the "Verification" section.
3.  Provide a summary of actions taken and confirmation of successful Next.js frontend setup.
4.  Once confirmed, proceed to the next file: `07_Frontend_Setup_UI_Libraries.md`.