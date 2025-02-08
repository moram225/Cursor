# 53_InventoryManagement_Backend_Services.md

**Core Feature:** Product and Inventory Management - Backend Services (NestJS)

**Purpose:** Define the NestJS services for managing Products and Stock Transactions, and generating basic inventory reports, interacting with Prisma ORM.

**Instructions for AI Agent:**

*   **Generate NestJS service classes:** `ProductService`, `StockTransactionService`, `InventoryReportService` (or functions within `ProductService` if preferred for reports).
*   For each service method (corresponding to API endpoints in `52_InventoryManagement_Backend_API.md`):
    *   Method signature with parameters and return types (TypeScript).
    *   Prisma Client interactions for CRUD operations, pagination, filtering, sorting, and searching.
    *   Implement logic for calculating current stock levels based on `FactStockTransactions`. This will likely involve aggregating `quantity_change` for each product and location.
    *   Error handling (Prisma exceptions, HTTP exceptions).
    *   Logging.
    *   Mapping Prisma entities to DTOs.
    *   Implement report generation logic for stock levels and stock valuation, including calculations of current stock and total value.

---

## Service Definitions:

### ProductService:

*   **createProduct(createProductDto: CreateProductDto): Promise<ProductDto>**: Prisma `create` for `DimProducts`.
*   **getProducts(queryParams: GetProductsQueryParams): Promise<PaginatedResponse<ProductDto[]>>**: Prisma `findMany`, `count` with pagination, filtering (on `productCategoryId`, `supplierId`, `isActive`, `isInventoryItem`, optional search), sorting.
*   **getProductById(productId: UUID): Promise<ProductDto>**: Prisma `findUnique` by `productId`. `NotFoundException` if not found.
*   **updateProduct(productId: UUID, updateProductDto: UpdateProductDto): Promise<ProductDto>**: Prisma `update` for `DimProducts`. `NotFoundException` if not found.
*   **deleteProduct(productId: UUID): Promise<void>**: Prisma `delete` for `DimProducts`. `NotFoundException` if not found.


### StockTransactionService:

*   **createStockTransaction(createStockTransactionDto: CreateStockTransactionDto): Promise<StockTransactionDto>**: Prisma `create` for `FactStockTransactions`.  *Consider business logic here - e.g., validation of `transaction_type` and `quantity_change` sign, potentially updating stock levels in memory or triggers (though for now, services will calculate levels on demand).*
*   **getStockTransactions(queryParams: GetStockTransactionsQueryParams): Promise<PaginatedResponse<StockTransactionDto[]>>**: Prisma `findMany`, `count` with pagination, filtering (on `productId`, `locationId`, `transactionType`, `transactionDate` range, optional search), sorting.
*   **getStockTransactionById(stockTransactionId: UUID): Promise<StockTransactionDto>**: Prisma `findUnique` by `stockTransactionId`. `NotFoundException` if not found.
*   **deleteStockTransaction(stockTransactionId: UUID): Promise<void>**: Prisma `delete` for `FactStockTransactions`. *(Review if DELETE is really needed).* `NotFoundException` if not found.


### InventoryReportService (or within ProductService):

*   **getStockLevels(queryParams: GetStockLevelsQueryParams): Promise<StockLevelDto[]>**:
    *   Implementation:
        *   Use Prisma to fetch all `DimProducts` (or filtered by `productCategoryId` if provided in `queryParams`).
        *   For each product, for each `DimLocation`, calculate `current_stock_quantity` by summing `quantity_change` from `FactStockTransactions` for that `product_id` and `location_id`.  This might involve a Prisma aggregation query for each product-location combination, or a more complex raw query or post-processing of transaction data in service. *Optimize for performance - consider caching stock levels if calculations are slow.*
        *   Return an array of `StockLevelDto` objects.

*   **getStockValuation(queryParams: GetStockValuationQueryParams): Promise<StockValuationDto[]>**:
    *   Implementation:
        *   Similar to `getStockLevels`, fetch `DimProducts` (or filtered).
        *   For each product, for each `DimLocation`, calculate `current_stock_quantity` as in `getStockLevels`.
        *   Multiply `current_stock_quantity` by `unit_cost` from `DimProducts` to get `total_value`.
        *   Return an array of `StockValuationDto` objects.

---