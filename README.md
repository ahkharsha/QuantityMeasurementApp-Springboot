# Quantity Measurement App

A Java-based Spring Boot REST API for converting, comparing, and calculating physical quantities, designed using a clean Controller-Service-Repository architecture.

## Features Implemented

### REST API Architecture
* **Web Controllers:** Upgraded the application to a modern web service using Spring REST controllers. The app now communicates using standard HTTP methods (POST, GET) and automatically translates incoming and outgoing data into clean JSON format.
* **Clean Data Transfer:** Created specific Data Transfer Objects (DTOs) like `QuantityInputDTO` to securely package and transport user inputs from the web into the application's core logic.

### Database Persistence
* **Automated Data Mapping:** Replaced old, manual SQL code with Spring Data JPA. The application now automatically maps Java objects directly to database tables without needing to write raw queries.
* **History Tracking:** Set up an embedded H2 database to automatically log every successful calculation and failed attempt, making it easy to track user activity and system health.

### Global Exception Handling
* **Centralized Error Catching:** Built a global error handler that watches over the entire application. If something goes wrong, it stops the server from crashing and instead sends a friendly, easy-to-read JSON error message back to the user.
* **Input Validation:** Added built-in checks to ensure users cannot submit empty values or fake measurement units, catching bad data before it even reaches the math logic.

### Service Layer Business Logic
* **Smart Conversions:** Built a dedicated service layer that automatically normalizes different units (like converting Feet to Inches) before doing any math, ensuring accurate additions, subtractions, and comparisons.
* **Defensive Math:** Added safety boundaries inside the logic to actively prevent impossible operations, like trying to divide by zero or trying to add a length (Feet) to a weight (Kilograms).

### Interactive API Documentation
* **Live Dashboard:** Integrated Swagger (OpenAPI) to automatically build a visual web dashboard. This allows anyone to see all available API endpoints, view exactly what data they require, and test them directly in the browser without needing extra tools.

### Automated Testing
* **Simulated Web Tests:** Used `MockMvc` to test the web controllers in isolation, ensuring the routing and error messages work perfectly.
* **Full Integration Tests:** Wrote deep, automated tests that spin up the entire server and fire real HTTP requests at the database, verifying that the controller, service, and database all talk to each other correctly under pressure.

## Tech Stack
* Java 21
* Spring Boot (REST, Data JPA, Validation)
* H2 Database (In-Memory)
* Swagger / OpenAPI (Documentation)
* JUnit 5 & Mockito (Testing)

## How to Run
1. Open the project in your **Spring Tool Suite (STS)** IDE.
2. In the **Boot Dashboard** (or Package Explorer), locate the `QuantityMeasurementApplication.java` file.
3. Right-click the file, select **Run As**, and click **Spring Boot App**.
4. Once the console says the server has started on port `8080`, open your web browser and go to `http://localhost:8080/swagger-ui.html` to view and test the API.
5. **To view the database:** Go to `http://localhost:8080/h2-console` and log in with the username `sa` (leave the password blank).
6. **To run tests:** Right-click the `src/test/java` folder, select **Run As**, and click **JUnit Test**.

## Complete API Endpoints Guide

All endpoints are prefixed with the base URL: `/api/v1/quantities`

### Calculations & Operations (POST)
| Endpoint Path | Description |
| :--- | :--- |
| `/compare` | Evaluates if two quantities are mathematically equal (e.g., 1 Foot == 12 Inches). |
| `/convert` | Converts a base quantity into your desired target unit. |
| `/add` | Adds two quantities together. |
| `/add-with-target-unit` | Adds two quantities and explicitly formats the final answer into a requested target unit. |
| `/subtract` | Subtracts the second quantity from the first. |
| `/subtract-with-target-unit`| Subtracts two quantities and explicitly formats the final answer into a requested target unit. |
| `/divide` | Divides the first quantity by the second. Includes safety checks to prevent division by zero. |

### History & Analytics (GET)
| Endpoint Path | Description |
| :--- | :--- |
| `/history/operation/{operation}`| Retrieves a list of past database records filtered by the exact operation type (e.g., ADD, COMPARE). |
| `/history/type/{type}` | Retrieves database history filtered by measurement category (e.g., LengthUnit, TemperatureUnit). |
| `/count/{operation}` | Returns the total count of *successful* executions for a specific operation. |
| `/history/errored` | Retrieves a specialized log of all operations that failed and generated an internal error. |