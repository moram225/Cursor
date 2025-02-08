# 52_InventoryManagement_Backend_API.md

**Core Feature:** Product and Inventory Management - Backend API Endpoints (NestJS)

**Purpose:** Define the RESTful API endpoints for managing Products and Stock Transactions in the backend NestJS application.

**Instructions for AI Agent:**

*   **Generate NestJS controller code** with the API endpoints defined below.
*   Use `/products` and `/stock-transactions` as base paths for controllers.
*   For each endpoint, generate:
    *   NestJS Controller decorator (`@Controller`)
    *   Route path decorators (`@Get`, `@Post`, `@Put`, `@Delete`)
    *   Method signature with parameters and types (TypeScript).
    *   Basic method body calling service methods (placeholders to services in `53_InventoryManagement_Backend_Services.md`).
    *   Appropriate HTTP status codes.
    *   Authentication (`@UseGuards(JwtAuthGuard)`).
    *   Basic validation using NestJS `ValidationPipe` and DTOs.

---

## API Endpoints:

### Product Endpoints (`/products`):

*   **POST /products**: Create a new Product.
    *   Method: `POST`
    *   Request Body: `CreateProductDto` (Define DTO with fields from `DimProducts` table - product_category_id, product_name, description, unit_cost, unit_price, unit_of_measure, is_active, is_inventory_item, reorder_level, reorder_quantity, supplier_id - with validation).
    *   Response: `ProductDto`, 201 Created.

*   **GET /products**: Get a list of Products (with pagination, filtering, search, category).
    *   Method: `GET`
    *   Query Parameters:
        *   Pagination: `page`, `pageSize`
        *   Filtering: `productCategoryId`, `supplierId`, `isActive`, `isInventoryItem`
        *   Search: (Consider search fields - product_name, description?)
        *   Sorting: (e.g., by product_name, unit_price)
    *   Response: `PaginatedResponse<ProductDto[]>`, 200 OK.

*   **GET /products/:productId**: Get a specific Product by ID.
    *   Method: `GET`
    *   Path Parameter: `productId` (UUID)
    *   Response: `ProductDto`, 200 OK.

*   **PUT /products/:productId**: Update an existing Product.
    *   Method: `PUT`
    *   Path Parameter: `productId` (UUID)
    *   Request Body: `UpdateProductDto` (Partial updates, validation).
    *   Response: `ProductDto`, 200 OK.

*   **DELETE /products/:productId**: Delete a Product.
    *   Method: `DELETE`
    *   Path Parameter: `productId` (UUID)
    *   Response: 204 No Content.


### Stock Transaction Endpoints (`/stock-transactions`):

*   **POST /stock-transactions**: Record a new Stock Transaction (Stock In, Stock Out, Adjustment).
    *   Method: `POST`
    *   Request Body: `CreateStockTransactionDto` (Define DTO with fields from `FactStockTransactions` table - product_id, location_id, transaction_type, quantity_change, batch_number, expiry_date, reference_document, notes, staff_id - validation, ensure `transaction_type` is ENUM and `quantity_change` sign is consistent with `transaction_type`).
    *   Response: `StockTransactionDto`, 201 Created.

*   **GET /stock-transactions**: Get a list of Stock Transactions (with pagination, filtering, search, date range, product, location, transaction type).
    *   Method: `GET`
    *   Query Parameters:
        *   Pagination: `page`, `pageSize`
        *   Filtering: `productId`, `locationId`, `transactionType`, `transactionDate` range
        *   Search: *(Consider search fields - product name, reference document, notes?)*
        *   Sorting: (e.g., by transaction_date, product_name)
    *   Response: `PaginatedResponse<StockTransactionDto[]>`, 200 OK.

*   **GET /stock-transactions/:stockTransactionId**: Get a specific Stock Transaction by ID.
    *   Method: `GET`
    *   Path Parameter: `stockTransactionId` (UUID)
    *   Response: `StockTransactionDto`, 200 OK.

*   **DELETE /stock-transactions/:stockTransactionId**:  *(Consider if DELETE is needed for stock transactions.  Perhaps only adjustments should be allowed, and history maintained? For now, include DELETE, but flag for review).*
    *   Method: `DELETE`
    *   Path Parameter: `stockTransactionId` (UUID)
    *   Response: 204 No Content.


### Inventory Reporting Endpoints (`/reports/inventory`):

*   **GET /reports/inventory/stock-levels**: Get current stock levels for all products (or filtered by product category/location).
    *   Method: `GET`
    *   Query Parameters (Optional):
        *   `productCategoryId`
        *   `locationId`
    *   Response: `StockLevelDto[]` (Define DTO with `product_id`, `product_name`, `location_id`, `location_name`, `current_stock_quantity`), 200 OK.
    *   Description: Returns current stock quantity for each product at each location (or filtered locations/categories).  *Service will need to calculate current stock based on `FactStockTransactions`.*

*   **GET /reports/inventory/stock-valuation**: Get stock valuation report.
    *   Method: `GET`
    *   Query Parameters (Optional):
        *   `locationId`
        *   `productCategoryId`
    *   Response: `StockValuationDto[]` (Define DTO with `product_id`, `product_name`, `location_id`, `location_name`, `current_stock_quantity`, `unit_cost`, `total_value` - `current_stock_quantity * unit_cost`), 200 OK.
    *   Description: Returns stock valuation (total cost of current inventory) for each product (or filtered). *Service to calculate `current_stock_quantity` and `total_value`.*

---