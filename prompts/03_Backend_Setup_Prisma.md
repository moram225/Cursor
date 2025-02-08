# 03_Backend_Setup_Prisma.md

**Purpose:**
This file guides the AI Agent to integrate Prisma ORM into the NestJS backend project. This includes installing Prisma CLI and client, initializing Prisma, setting up the Prisma schema, and generating the Prisma client.

**Instructions:**

1.  **Install Prisma CLI as a development dependency:**
    ```bash
    npm install prisma --save-dev
    ```

2.  **Install Prisma Client:**
    ```bash
    npm install @prisma/client
    ```

3.  **Initialize Prisma in the project:**
    *   Run the Prisma initialization command to set up Prisma schema and related configurations. This will create a `prisma` folder in the project root containing the `schema.prisma` file and `.env` file if it doesn't exist.

    ```bash
    npx prisma init --datasource-provider postgresql
    ```

4.  **Configure Prisma Schema (`schema.prisma`):**
    *   Open the `prisma/schema.prisma` file.
    *   **Update the `datasource` block:** Ensure the `url` field in the `datasource "db"` block is correctly referencing the `DATABASE_URL` environment variable you set in `02_Backend_Setup_PostgreSQL.md`. It should look like this:

    ```prisma
    datasource db {
      provider = "postgresql"
      url      = env("DATABASE_URL")
    }
    ```

    *   **Basic Data Model (Initial - to be expanded later):** For now, define a very basic data model in `schema.prisma` just to verify Prisma setup.  Let's create a simple `Tenant` model. We will expand this schema significantly in a later step (`10_Database_Schema_Design.md`).

    ```prisma
    model Tenant {
      id        Int      @id @default(autoincrement())
      createdAt DateTime @default(now())
      updatedAt DateTime @updatedAt
      name      String
    }
    ```

5.  **Generate Prisma Client:**
    *   Run the Prisma generate command to generate the Prisma Client based on your `schema.prisma`. This will create the Prisma Client code in `node_modules/@prisma/client`.

    ```bash
    npx prisma generate
    ```

6.  **Create a Prisma Service in NestJS:**
    *   Inside the `src/common` folder, create a new folder named `prisma`.
    *   Inside `src/common/prisma`, create a file named `prisma.service.ts`.
    *   Implement a NestJS service that instantiates the Prisma Client and handles connection lifecycle (e.g., `onModuleInit` to connect, `onModuleDestroy` to disconnect).

    ```typescript  typescript
    // src/common/prisma/prisma.service.ts
    import { Injectable, OnModuleInit, OnModuleDestroy } from '@nestjs/common';
    import { PrismaClient } from '@prisma/client';

    @Injectable()
    export class PrismaService extends PrismaClient implements OnModuleInit, OnModuleDestroy {
      async onModuleInit() {
        await this.$connect();
      }

      async onModuleDestroy() {
        await this.$disconnect();
      }
    }
    ```

7.  **Export and Import Prisma Service:**
    *   In `src/common/common.module.ts` (you might need to create this module if it doesn't exist within `src/common`), declare and export the `PrismaService`.
    *   Import `CommonModule` into your `AppModule` (`src/app.module.ts`) to make the `PrismaService` available throughout your application.

    ```typescript  typescript
    // src/common/common.module.ts
    import { Module } from '@nestjs/common';
    import { PrismaService } from './prisma/prisma.service';

    @Module({
      providers: [PrismaService],
      exports: [PrismaService], // Export PrismaService to be used in other modules
    })
    export class CommonModule {}
    ```

    ```typescript  typescript
    // src/app.module.ts
    import { Module } from '@nestjs/common';
    import { AppController } from './app.controller';
    import { AppService } from './app.service';
    import { CommonModule } from './common/common.module'; // Import CommonModule

    @Module({
      imports: [CommonModule], // Add CommonModule to imports
      controllers: [AppController],
      providers: [AppService],
    })
    export class AppModule {}
    ```

**Context:**

*   We are integrating Prisma ORM to handle database interactions in our NestJS backend.
*   Prisma provides type-safe database access and simplifies database migrations and schema management.
*   This setup lays the foundation for defining our database models and performing database operations.

**Rules and Constraints:**

*   Use Prisma ORM for database interactions.
*   Use the Prisma CLI for initialization and schema generation.
*   Define the initial Prisma schema in `prisma/schema.prisma`.
*   Create a NestJS `PrismaService` to encapsulate Prisma Client and manage connections.
*   Export and import `PrismaService` using NestJS modules to make it accessible.

**Details:**

*   Prisma Client will be generated in `node_modules/@prisma/client`.
*   Prisma Schema file: `prisma/schema.prisma`
*   Prisma Service file: `src/common/prisma/prisma.service.ts`
*   Common Module file: `src/common/common.module.ts`

**Verification:**

*   After executing these instructions, the AI Agent should confirm:
    *   Installation of `prisma` and `@prisma/client` npm packages.
    *   Successful Prisma initialization (`npx prisma init`).
    *   Correct configuration of `DATABASE_URL` in `prisma/schema.prisma`.
    *   Creation of the basic `Tenant` model in `prisma/schema.prisma`.
    *   Successful Prisma client generation (`npx prisma generate`).
    *   Creation of `PrismaService` in `src/common/prisma/prisma.service.ts` with connection logic.
    *   Creation of `CommonModule` in `src/common/common.module.ts` and export of `PrismaService`.
    *   Import of `CommonModule` in `AppModule`.
    *   **(Optional but recommended):**  A basic test to query the database using the Prisma Client (e.g., in `AppService`) to confirm successful connection.  This would involve creating a Tenant record and fetching it.  We can add instructions for this if needed.

**Next Steps (for AI Agent):**

1.  Execute the commands and steps outlined in the "Instructions" section.
2.  Verify each step as described in the "Verification" section.
3.  Provide a summary of actions taken and confirmation of successful Prisma setup and integration.
4.  Once confirmed, proceed to the next file: `04_Backend_Setup_Authentication.md`.