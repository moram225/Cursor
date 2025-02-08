# 02_Backend_Setup_PostgreSQL.md

**Purpose:**
This file instructs the AI Agent to set up PostgreSQL for the Veterinary PMS backend. This includes ensuring PostgreSQL is installed, configuring a development database, and setting up connection environment variables.

**Instructions:**

1.  **Ensure PostgreSQL is installed and running:**
    *   **Check if PostgreSQL is installed:**  The AI Agent should first check if PostgreSQL is already installed and accessible in the environment.  This might involve checking for the `psql` command in the system's PATH.
    *   **If not installed, provide installation instructions:** If PostgreSQL is not found, provide general instructions for installing PostgreSQL based on common operating systems (Windows, macOS, Linux).  *(Initially, just assume PostgreSQL is expected to be installed.  If the AI Agent has trouble connecting later, we can refine this to include more detailed installation steps for different OS).*
    *   **Ensure PostgreSQL server is running:** Verify that the PostgreSQL server is running and accepting connections.

2.  **Create a development database:**
    *   Use the `psql` command-line tool or a PostgreSQL administration tool to create a new PostgreSQL database named `veterinary_pms_dev`.
    *   Use a default user (e.g., `postgres`) and password (if applicable - often default is no password for local development, but best to set a simple one for consistency).
    *   If using `psql` command-line:

    ```bash
    psql -U postgres -c "CREATE DATABASE veterinary_pms_dev;"
    ```

    *(Note: Adjust `-U postgres` if you have a different default PostgreSQL user. You might need to handle password prompts if set for the 'postgres' user)*

3.  **Set up database connection environment variables:**
    *   Open the `.env` file in the root of the `veterinary-pms-backend` project.
    *   Add the following environment variables to configure the database connection.  *(Assume default PostgreSQL settings for local development initially.  These can be adjusted later if needed)*:

    ```env
    PORT=3000
    DATABASE_URL="postgresql://postgres:your_dev_password@localhost:5432/veterinary_pms_dev?schema=public"
    # Replace "your_dev_password" with your actual development PostgreSQL password if set. If no password, leave it empty after the colon.
    # Example with no password: DATABASE_URL="postgresql://postgres:@localhost:5432/veterinary_pms_dev?schema=public"
    ```

    *   **Important:** If you set a password for your `postgres` user in your local PostgreSQL setup, replace `your_dev_password` with that password in the `DATABASE_URL`. If there's no password for the `postgres` user, you can leave it as `postgresql://postgres:@localhost:5432/veterinary_pms_dev?schema=public`.

**Context:**

*   We are setting up PostgreSQL as the database for our Veterinary PMS backend.
*   This setup is for the development environment.
*   Prisma ORM (configured in the next step) will use these connection details to interact with the database.

**Rules and Constraints:**

*   Use PostgreSQL as the database system.
*   Create a database named `veterinary_pms_dev` for development.
*   Use environment variables (in `.env` file) to store database connection details.
*   Assume default PostgreSQL port (5432) and localhost for development.
*   For simplicity initially, assume using the `postgres` user.

**Details:**

*   Database Name: `veterinary_pms_dev`
*   Default PostgreSQL port: `5432`
*   Host: `localhost`
*   User: `postgres` (default PostgreSQL user)
*   Password:  *(configurable via `DATABASE_URL` environment variable.  Assume no password initially for simplicity, but user should be instructed to set one for production)*

**Verification:**

*   After executing these instructions, the AI Agent should confirm:
    *   Verification that PostgreSQL is running or instructions provided if not found.
    *   Successful creation of the `veterinary_pms_dev` database.
    *   Addition of the `DATABASE_URL` environment variable to the `.env` file (correctly formatted).
    *   Potentially, a successful connection to the PostgreSQL database can be tested (more thoroughly tested in the Prisma setup in the next file).

**Next Steps (for AI Agent):**

1.  Execute the commands and steps outlined in the "Instructions" section.
2.  Verify each step as described in the "Verification" section.
3.  Provide a summary of actions taken and confirmation of successful PostgreSQL setup.
4.  Once confirmed, proceed to the next file: `03_Backend_Setup_Prisma.md`.