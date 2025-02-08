# 07_Frontend_Setup_UI_Libraries.md

**Purpose:**

This file instructs the AI agent to set up the necessary UI component libraries for the Veterinary PMS frontend. We will be using a combination of libraries to leverage templates, pre-built components, and customization options.

**Instructions:**

1.  **Install MUI Core and Joy UI:**
    *   Using `npm` or `yarn`, add MUI Core and Joy UI and their dependencies to the Next.js frontend project.

        ```bash
        npm install @mui/material @mui/joy @emotion-react @emotion-styled
        # or
        yarn add @mui/material @mui/joy @emotion-react @emotion-styled
        ```

2.  **Install Shadcn UI:**
    *   Follow the Shadcn UI installation instructions for Next.js projects. This typically involves using `shadcn-ui` CLI to add components as needed.
    *   Initialize Shadcn UI within your Next.js project if you haven't already:

        ```bash
        npx shadcn-ui@latest init
        ```
    *   Configure Shadcn UI's `tailwind.config.js` and `globals.css` as per Shadcn UI documentation to ensure proper Tailwind CSS integration.

3.  **Configure Theme (Initial - Basic MUI Theme):**
    *   For initial setup, let's configure a basic MUI theme provider in `_app.tsx` (or `_app.js`) to enable MUI styling across the application.  We can customize the theme later.
    *   Create a basic theme in `src/theme.ts` (or `src/theme/index.ts` if you prefer an index file in a theme directory):

        ```typescript
        // src/theme.ts (or src/theme/index.ts)
        import { createTheme } from '@mui/material/styles';
        import { inter } from './font'; // Assuming you are using the 'inter' font from next/font as in Next.js setup

        const theme = createTheme({
          typography: {
            fontFamily: inter.style.fontFamily,
          },
          // ... you can add more theme customizations here later (colors, etc.) ...
        });

        export default theme;
        ```

    *   Update `_app.tsx` (or `_app.js`) to use the `ThemeProvider` from `@mui/material` and import your theme:

        ```typescript jsx
        // _app.tsx
        import React from 'react';
        import { ThemeProvider } from '@mui/material/styles';
        import theme from '../src/theme'; // or '../src/theme/index'
        import '../styles/globals.css'; // Ensure your global styles are imported

        function MyApp({ Component, pageProps }) {
          return (
            <ThemeProvider theme={theme}>
              <Component {...pageProps} />
            </ThemeProvider>
          );
        }

        export default MyApp;
        ```

4.  **Verify Installation:**
    *   Run the Next.js development server (`npm run dev` or `yarn dev`).
    *   Create a simple test page (e.g., `pages/test-ui.tsx`) and import a component from each library (e.g., `<Button>` from `@mui/material`, `<Button>` from `@mui/joy`, a Shadcn UI component like `<Button>`). Render these components on the test page and ensure they are displayed without errors and with basic styling applied, indicating that the libraries are correctly installed and the theme is applied.

**Expected Outcome:**

*   MUI Core, Joy UI, and Shadcn UI libraries are successfully installed in the Next.js project.
*   A basic MUI theme is set up and applied to the application.
*   Verification steps confirm that the UI libraries are working correctly.

**Post-Verification:**

*   Provide confirmation that all UI libraries are installed and the basic theme is set up correctly.
*   Report any errors encountered during installation or verification.

**Next File:**

*   Proceed to the next file in the `00_Setup_Control_Center.md` execution plan after successful completion of this file.