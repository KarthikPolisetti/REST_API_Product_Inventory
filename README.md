REST_API_Product_Inventory
A Spring Boot RESTful API for managing a product inventory system with MySQL, supporting CRUD operations, pagination, sorting, filtering, custom exception handling, and Swagger documentation.

**Objective**
Create a RESTful API using Spring Boot to manage a product inventory system with the following requirements:

Use Spring Boot to create a RESTful API project.
Create a Product model with fields: id, name, category, quantity, and price.
Use Spring Data JPA with a MySQL database.
Implement RESTful endpoints for CRUD operations.
Test endpoints using Postman or Swagger UI.
Use appropriate HTTP methods: POST, GET, PUT, DELETE.
Handle exceptions and send appropriate response codes.
Bonus: Add pagination, sorting, filtering by category or price range, custom exception handling, validation annotations, and Swagger for API documentation.

Setup Instructions
Prerequisites

Java 17 or higher
MySQL 8.0 or higher
Maven
Git
Postman or Swagger UI (for testing)

Clone the Repository
git clone https://github.com/KarthikPolisetti/REST_API_Product_Inventory
cd REST_API_Product_Inventory

Configure MySQL

Create a MySQL database:CREATE DATABASE inventory_db;


Run the provided schema script (inventory/schema.sql):SOURCE inventory/schema.sql;


Update src/main/resources/application.properties with your MySQL credentials:spring.datasource.url=jdbc:mysql://localhost:3306/inventory_db
spring.datasource.username=root
spring.datasource.password=your_password
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect



Build and Run
mvn clean install
mvn spring-boot:run

The application runs on http://localhost:8080.
Access Swagger UI

Open http://localhost:8080/swagger-ui/index.html to view API documentation and test endpoints.

Database Schema
The MySQL schema (inventory/schema.sql) creates a products table:
CREATE DATABASE IF NOT EXISTS inventory_db;
USE inventory_db;

CREATE TABLE products (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    category VARCHAR(100) NOT NULL,
    quantity INT NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

API Endpoints
All endpoints are prefixed with /api/products.



Method
Endpoint
Description



GET
/api/products
Retrieve all products (supports pagination, sorting, and filtering)


GET
/api/products/{id}
Retrieve a product by ID


POST
/api/products
Create a new product


PUT
/api/products/{id}
Update an existing product


DELETE
/api/products/{id}
Delete a product


Query Parameters (GET /api/products)

page: Page number for pagination (default: 0)
size: Number of items per page (default: 10)
sort: Field to sort by (e.g., name, price, quantity) and direction (e.g., asc, desc)
category: Filter by category (e.g., Electronics)
minPrice: Filter by minimum price
maxPrice: Filter by maximum price

Example: GET /api/products?page=0&size=5&sort=price,desc&category=Electronics&minPrice=100&maxPrice=1000
Sample Requests
Use Postman, cURL, or Swagger UI to test the endpoints.

Get All Products (with pagination, sorting, and filtering):
curl "http://localhost:8080/api/products?page=0&size=5&sort=price,desc&category=Electronics"

Response (200 OK):
{
    "content": [
        {
            "id": 1,
            "name": "Laptop",
            "category": "Electronics",
            "quantity": 10,
            "price": 999.99,
            "createdAt": "2025-07-06T12:00:00"
        },
        ...
    ],
    "pageable": { ... },
    "totalPages": 2,
    "totalElements": 8
}


Get Product by ID:
curl http://localhost:8080/api/products/1

Response (200 OK):
{
    "id": 1,
    "name": "Laptop",
    "category": "Electronics",
    "quantity": 10,
    "price": 999.99,
    "createdAt": "2025-07-06T12:00:00"
}

Error (404 Not Found):
{
    "status": 404,
    "error": "Not Found",
    "message": "Product with ID 1 not found"
}


Create a Product:
curl -X POST http://localhost:8080/api/products \
-H "Content-Type: application/json" \
-d '{"name":"Laptop","category":"Electronics","quantity":10,"price":999.99}'

Response (201 Created):
{
    "id": 1,
    "name": "Laptop",
    "category": "Electronics",
    "quantity": 10,
    "price": 999.99,
    "createdAt": "2025-07-06T12:00:00"
}

Error (400 Bad Request, e.g., invalid input):
{
    "status": 400,
    "error": "Bad Request",
    "message": "Name cannot be empty"
}


Update a Product:
curl -X PUT http://localhost:8080/api/products/1 \
-H "Content-Type: application/json" \
-d '{"name":"Updated Laptop","category":"Electronics","quantity":8,"price":1099.99}'

Response (200 OK):
{
    "id": 1,
    "name": "Updated Laptop",
    "category": "Electronics",
    "quantity": 8,
    "price": 1099.99,
    "createdAt": "2025-07-06T12:00:00"
}


Delete a Product:
curl -X DELETE http://localhost:8080/api/products/1

Response (204 No Content): No bodyError (404 Not Found):
{
    "status": 404,
    "error": "Not Found",
    "message": "Product with ID 1 not found"
}



Bonus Features

Pagination and Sorting: Implemented using Spring Data JPA’s Pageable. Example: GET /api/products?page=0&size=5&sort=price,desc.
Filtering: Filter by category, minPrice, and maxPrice using query parameters.
Custom Exception Handling: Custom exceptions (e.g., ProductNotFoundException) with appropriate HTTP status codes (e.g., 404, 400).
Validation: Uses @NotNull, @NotEmpty, and @Positive annotations for input validation.
Swagger Documentation: Available at http://localhost:8080/swagger-ui/index.html.

Testing

Use Swagger UI (http://localhost:8080/swagger-ui/index.html) for interactive testing.
Alternatively, use Postman or cURL with the sample requests above.
Ensure the MySQL database is running before testing.

Project Structure

src/main/java/com/example/inventory:
model/Product.java: Product entity with id, name, category, quantity, price.
repository/ProductRepository.java: Spring Data JPA repository.
controller/ProductController.java: REST controller for CRUD operations.
service/ProductService.java: Business logic with pagination and filtering.
exception/ProductNotFoundException.java: Custom exception for 404 errors.


src/main/resources:
application.properties: MySQL configuration.
schema.sql: Database schema script.


README.md: This file.

Notes

Ensure MySQL is running and configured correctly.
Update the application.properties file with your MySQL credentials.
The project uses Spring Boot 3.x and Maven for dependency management.
For production, consider adding security (e.g., Spring Security) and additional validation.
