# 09_Frontend_Setup_API_Client.md

**Purpose:**
This file guides the AI Agent to set up an API client service in the Next.js frontend project. This service will handle communication with the NestJS backend API using `axios` for making HTTP requests.

**Instructions:**

1.  **Install `axios`:**
    *   Install `axios` as a dependency in your `veterinary-pms-frontend` project using npm:

    ```bash
    npm install axios
    ```

2.  **Create `ApiService` in `src/services` folder:**
    *   Inside the `src/services` directory (which you should have created in `06_Frontend_Setup_NextJS.md`), create a new file named `api.service.ts`.
    *   Implement an `ApiService` class in `api.service.ts` with the following functionalities:

    *   **Base URL Configuration:**
        *   Define a constant or variable for the base URL of your backend API. For now, hardcode a placeholder URL.  *(In a later step, we will configure using environment variables for different environments like development, production, etc.)*

        ```typescript
        // src/services/api.service.ts
        const API_BASE_URL = 'http://localhost:3000'; // Placeholder - Backend API URL.  Will be configured via env variables later.
        ```

    *   **Generic `request` method:**
        *   Implement a generic `request` method that will handle making HTTP requests (GET, POST, PUT, DELETE, etc.) using `axios`. This method should accept:
            *   `method`: HTTP method (e.g., 'get', 'post', 'put', 'delete').
            *   `url`:  Endpoint URL (relative to the `API_BASE_URL`).
            *   `data` (optional): Request body data for methods like POST and PUT.
            *   `config` (optional):  Axios request configuration object (for headers, etc.).

        *   The `request` method should:
            *   Construct the full API URL by combining `API_BASE_URL` and the provided `url`.
            *   Use `axios` to make the HTTP request with the specified method, URL, data, and config.
            *   Return the `axios` response (or handle errors and return a structured error response).

    *   **Basic Error Handling:**
        *   Include basic error handling within the `request` method. For example, catch `axios` errors and return a structured error object or re-throw the error.  For now, a simple `console.error` and re-throw is sufficient. More robust error handling can be added later.

    *   **Export `ApiService`:**
        *   Export the `ApiService` class so it can be imported and used in other parts of the frontend application.

    *   Example `api.service.ts` (basic structure):

    ```typescript  typescript
    // src/services/api.service.ts
    import axios, { AxiosRequestConfig, AxiosResponse } from 'axios';

    const API_BASE_URL = 'http://localhost:3000'; // Placeholder - Backend API URL

    class ApiService {
      async request<T, R = AxiosResponse<T>>(method: string, url: string, data?: any, config?: AxiosRequestConfig): Promise<R> {
        try {
          const response: R = await axios.request<T, R>({
            method: method,
            url: `${API_BASE_URL}${url}`,
            data: data,
            ...config,
          });
          return response;
        } catch (error: any) {
          console.error('API Request Error:', error); // Basic error logging for now
          throw error; // Re-throw error for component-level handling
        }
      }

      // Example helper methods for common HTTP methods (optional, can be added later or used directly via request method)
      async get<T, R = AxiosResponse<T>>(url: string, config?: AxiosRequestConfig): Promise<R> {
        return this.request<T, R>('get', url, undefined, config);
      }

      async post<T, R = AxiosResponse<T>>(url: string, data?: any, config?: AxiosRequestConfig): Promise<R> {
        return this.request<T, R>('post', url, data, config);
      }

      async put<T, R = AxiosResponse<T>>(url: string, data?: any, config?: AxiosRequestConfig): Promise<R> {
        return this.request<T, R>('put', url, data, config);
      }

      async delete<T, R = AxiosResponse<T>>(url: string, config?: AxiosRequestConfig): Promise<R> {
        return this.request<T, R>('delete', url, undefined, config);
      }
    }

    export default new ApiService(); // Export as a singleton instance
    ```

    *   **Important:**  The `API_BASE_URL` is currently hardcoded.  Remember that in a real-world application, you should configure this using environment variables (e.g., in `.env.local` for development, and environment variables for production deployments) to easily switch between different backend environments. We will address environment variable configuration more formally in a later step.

3.  **Example Usage (Verification - Optional):**
    *   **(Optional):** To verify the `ApiService`, you could try a basic API call (if you have a "Hello World" endpoint running on your NestJS backend from `01_Backend_Setup_NestJS.md`, or any public API).  For example, you could add a test function in your `src/app/page.tsx` to call a test endpoint (if available) or a simple public API endpoint using the `ApiService.get()` method.  This is not strictly required now, but can help verify connectivity if you have a backend running.


**Context:**

*   We are creating an API client service to handle communication between the Next.js frontend and the NestJS backend.
*   `axios` is being used as the HTTP client library.
*   The `ApiService` will encapsulate API request logic and provide a reusable way to make API calls from frontend components.

**Rules and Constraints:**

*   Use `axios` for making HTTP requests.
*   Create an `ApiService` class in `src/services/api.service.ts`.
*   Implement a generic `request` method in `ApiService` to handle various HTTP methods.
*   Include basic error handling in the `request` method.
*   Hardcode a placeholder `API_BASE_URL` for now (e.g., `'http://localhost:3000'`).
*   Export `ApiService` as a singleton instance.

**Details:**

*   API Client Service file: `src/services/api.service.ts`
*   HTTP Client Library: `axios`
*   Placeholder API Base URL: `'http://localhost:3000'` (to be replaced with environment variable later)

**Verification:**

*   After executing these instructions, the AI Agent should confirm:
    *   Installation of `axios` npm package.
    *   Creation of `ApiService` class in `src/services/api.service.ts`.
    *   Implementation of the `request` method in `ApiService` with `axios` and basic error handling.
    *   Definition of `API_BASE_URL` constant (placeholder) in `api.service.ts`.
    *   Export of `ApiService` as a singleton instance.
    *   **(Optional):** If you performed the optional verification step, confirm successful execution of a test API call using `ApiService` (if applicable).

**Next Steps (for AI Agent):**

1.  Execute the commands and steps outlined in the "Instructions" section.
2.  Verify each step as described in the "Verification" section.
3.  Provide a summary of actions taken and confirmation of successful API Client setup.
4.  Once confirmed, proceed to the next file: `10_Database_Schema_Design.md`.