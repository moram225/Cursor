# 01_Backend_Setup_NestJS.md

**Purpose:**
This file guides the AI Agent to set up the basic NestJS backend project for the Veterinary PMS. This includes initializing the project, setting up the project structure, and installing essential dependencies.

**Instructions:**

1.  **Create a new NestJS project:**
    *   Use the NestJS CLI to create a new project named `veterinary-pms-backend`.
    *   Use npm as the package manager.
    *   Select `TypeScript` as the language.
    *   When prompted for the project structure, choose the default structure.

    ```bash
    npm install -g @nestjs/cli
    nest new veterinary-pms-backend
    cd veterinary-pms-backend
    ```

2.  **Install core NestJS dependencies:**
    *   Ensure the following core NestJS modules are installed. If not already present in `package.json` and `node_modules`, install them using npm:
        *   `@nestjs/core`
        *   `@nestjs/common`
        *   `@nestjs/platform-express`
        *   `reflect-metadata`
        *   `rxjs`

    ```bash
    npm install @nestjs/core @nestjs/common @nestjs/platform-express reflect-metadata rxjs
    ```

3.  **Basic Project Structure Setup:**
    *   Create the following folders within the `src` directory to organize the backend logically:
        *   `auth`:  For authentication related modules and files.
        *   `tenants`: For multi-tenancy related modules and files.
        *   `patients`: For patient management features.
        *   `clients`: For client management features.
        *   `billing`: For billing and financial features.
        *   `inventory`: For inventory and supply chain features.
        *   `reporting`: For reporting and analytics features.
        *   `communication`: For communication tools features.
        *   `superadmin`: For SuperAdmin specific features and modules.
        *   `config`: For configuration files and environment variables.
        *   `common`: For shared utilities, services, and DTOs.

4.  **Create a basic "Hello World" Controller and Service (optional, for verification):**
    *   Inside the `src` folder, modify the `app.controller.ts`, `app.service.ts`, and `app.module.ts` to create a simple "Hello World" endpoint for initial testing. This is not strictly required but can help verify the basic setup.  The default generated files are usually sufficient for a basic test.

5.  **Initial Configuration (environment variables):**
    *   Create a `.env` file in the root of the `veterinary-pms-backend` directory.
    *   Add a basic environment variable for the application port:

    ```env
    PORT=3000
    ```

**Context:**

*   We are setting up the backend for the Veterinary PMS using NestJS, a progressive Node.js framework.
*   This is the first step in building the backend infrastructure.
*   Subsequent files will build upon this foundation to add database integration, authentication, and core features.

**Rules and Constraints:**

*   Use NestJS CLI for project creation.
*   Use npm as the package manager.
*   Use TypeScript as the programming language.
*   Adhere to NestJS project structure conventions where applicable.
*   Create the specified folder structure within the `src` directory.
*   Keep the initial setup minimal and focused on project initialization and core dependencies.

**Details:**

*   Project name: `veterinary-pms-backend`
*   Package manager: `npm`
*   Language: `TypeScript`
*   Port: `3000` (configurable via `.env` file)

**Verification:**

*   After executing these instructions, the AI Agent should confirm:
    *   Successful creation of the NestJS project named `veterinary-pms-backend`.
    *   Installation of core NestJS dependencies.
    *   Creation of the specified folder structure within the `src` directory.
    *   Creation of a `.env` file with `PORT=3000`.

**Next Steps (for AI Agent):**

1.  Execute the commands and steps outlined in the "Instructions" section.
2.  Verify each step as described in the "Verification" section.
3.  Provide a summary of actions taken and confirmation of successful setup.
4.  Once confirmed, proceed to the next file: `02_Backend_Setup_PostgreSQL.md`.