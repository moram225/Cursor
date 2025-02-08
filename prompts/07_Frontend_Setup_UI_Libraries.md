# 07_Frontend_Setup_UI_Libraries.md

**Purpose:**
This file guides the AI Agent to integrate the specified UI libraries (Shadcn UI, MUI Core, Joy UI Templates, and Planby React Calendar Timeline) into the Next.js frontend project. This includes installing the libraries and setting up basic configurations.

**Instructions:**

1.  **Install Shadcn UI:**
    *   Follow the Shadcn UI installation instructions for Next.js (App Router). Refer to the official Shadcn UI documentation for the most up-to-date instructions: [https://ui.shadcn.com/docs/installation/nextjs](https://ui.shadcn.com/docs/installation/nextjs).
    *   Typically, this involves using `shadcn-ui/init` and then using the `shadcn-ui/cli` to add components as needed.
    *   Initialize Shadcn UI within your `veterinary-pms-frontend` project:

    ```bash
    npx shadcn-ui@latest init
    ```
    *   Accept the default options during initialization (style: `default`, base color: `slate`, CSS variables: `yes`, color palette: `slate`, `utils.ts` location: `src/lib`, import alias: `@/components`).  *(If defaults are different, adjust instructions accordingly.)*

2.  **Install MUI Core (MUI v5 or later):**
    *   Install MUI Core and its peer dependencies using npm:

    ```bash
    npm install @mui/material @emotion/react @emotion/styled
    ```

3.  **Install Joy UI Templates (Joy UI is part of MUI):**
    *   Joy UI is part of MUI, so you likely already installed core dependencies with the previous step.  However, if you need specific Joy UI *templates* or components beyond the core, you might need to install `@mui/joy`.  Let's install `@mui/joy` explicitly to ensure Joy UI components are available:

    ```bash
    npm install @mui/joy
    ```

4.  **Install Planby React Calendar Timeline:**
    *   Install the `react-calendar-timeline` library and its dependencies:

    ```bash
    npm install react-calendar-timeline moment
    npm install @types/react-calendar-timeline @types/moment # Optional: Install type definitions if needed
    ```

5.  **Configure Tailwind CSS for potential conflicts (if needed):**
    *   Shadcn UI is based on Tailwind CSS. MUI also has its styling system.  In most cases, they can coexist. However, if you encounter any CSS conflicts or styling issues, you might need to adjust Tailwind CSS configuration or component styling to ensure proper integration.  *(For now, let's assume they will coexist reasonably well with default configurations. If styling conflicts arise later, we can add more specific configuration adjustments here.)*

6.  **Basic Component Import Test (Optional Verification):**
    *   **(Optional):** To verify that the UI libraries are installed correctly, you can try importing a basic component from each library in your `src/app/page.tsx` (or any component file) and see if they render without errors.  For example:

    ```tsx  typescript
    // src/app/page.tsx (Example - for verification only)
    import { Button } from '@/components/ui/button' // Shadcn UI Button (adjust import path if needed)
    import { Button as MUIButton } from '@mui/material'; // MUI Button (rename to avoid conflict)
    import ButtonJoy from '@mui/joy/Button'; // Joy UI Button
    import Timeline from 'react-calendar-timeline' // Planby Timeline

    export default function HomePage() {
      return (
        <div>
          <h1>Welcome to Veterinary PMS</h1>
          <Button>Shadcn Button</Button>
          <MUIButton variant="contained">MUI Button</MUIButton>
          <ButtonJoy color="primary">Joy UI Button</ButtonJoy>
          {/* Basic Timeline example - might need more props for full render */}
          <Timeline groups={[]} items={[]} defaultTimeStart={Date.now()} defaultTimeEnd={Date.now() + 3600 * 1000}/> 
        </div>
      )
    }
    ```
    *   **(Important):** This component import test in `page.tsx` is purely for verification. You can remove or comment out this test code after you confirm that the libraries are installed and importable.  You don't need to keep these test components in your actual application code.


**Context:**

*   We are integrating UI libraries to provide pre-built components and styling for our frontend, speeding up development and ensuring a consistent look and feel.
*   We are integrating Shadcn UI (for utility-first components), MUI (Core and Joy UI for a comprehensive component library), and Planby React Calendar Timeline (for appointment scheduling and visualization).

**Rules and Constraints:**

*   Integrate Shadcn UI, MUI Core, Joy UI Templates, and Planby React Calendar Timeline.
*   Follow the official installation instructions for each library.
*   Use `npm` for package installation.
*   Perform basic verification to ensure libraries are installed and importable.
*   Address any CSS conflicts or styling issues that may arise during integration (if any).

**Details:**

*   UI Libraries to install: Shadcn UI, MUI Core (`@mui/material`), Joy UI (`@mui/joy`), `react-calendar-timeline`, `moment`.
*   Refer to official documentation for each library for specific installation and usage details.
*   Shadcn UI Installation Doc: [https://ui.shadcn.com/docs/installation/nextjs](https://ui.shadcn.com/docs/installation/nextjs)

**Verification:**

*   After executing these instructions, the AI Agent should confirm:
    *   Successful initialization of Shadcn UI (`npx shadcn-ui@latest init`).
    *   Installation of `@mui/material`, `@emotion/react`, `@emotion/styled`, `@mui/joy`, `react-calendar-timeline`, and `moment` npm packages.
    *   **(Optional):** Successful rendering of basic components from each library in `src/app/page.tsx` (or a test component).
    *   Confirmation that UI libraries are ready to be used in the project.

**Next Steps (for AI Agent):**

1.  Execute the commands and steps outlined in the "Instructions" section.
2.  Verify each step as described in the "Verification" section.
3.  Provide a summary of actions taken and confirmation of successful UI Libraries integration.
4.  Once confirmed, proceed to the next file: `08_Frontend_Setup_Routing.md`.