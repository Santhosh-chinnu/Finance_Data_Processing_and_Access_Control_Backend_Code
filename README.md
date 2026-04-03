Finance Data Processing and Access Control Backend
Overview
A comprehensive, production-ready backend system for finance dashboard management with role-based access control, financial record keeping, and advanced analytics.
Built using Java Spring Boot and MySQL, this system provides secure REST APIs for managing users, financial transactions, and real-time dashboard analytics.
Project Structure:
finance-backend/
├── pom.xml
├── src/
│   └── main/
│       ├── java/com/finance/
│       │   ├── FinanceBackendApplication.java
│       │
│       │   ├── config/
│       │   │   └── SecurityConfig.java
│       │
│       │   ├── controller/
│       │   │   ├── AuthController.java
│       │   │   ├── DashboardController.java
│       │   │   ├── FinancialRecordController.java
│       │   │   └── UserController.java
│       │
│       │   ├── dto/
│       │   │   ├── request/
│       │   │   │   ├── FilterRequest.java
│       │   │   │   ├── FinancialRecordRequest.java
│       │   │   │   ├── LoginRequest.java
│       │   │   │   └── UserRegistrationRequest.java
│       │   │   │
│       │   │   ├── response/
│       │   │   │   ├── AuthResponse.java
│       │   │   │   ├── FinancialRecordResponse.java
│       │   │   │   └── UserResponse.java
│       │   │   │
│       │   │   └── DashboardSummaryDTO.java
│       │
│       │   ├── exception/
│       │   │   ├── AccessDeniedException.java
│       │   │   ├── GlobalExceptionHandler.java
│       │   │   └── ResourceNotFoundException.java
│       │
│       │   ├── model/
│       │   │   ├── enums/
│       │   │   │   ├── RecordType.java
│       │   │   │   ├── UserRole.java
│       │   │   │   └── UserStatus.java
│       │   │   ├── FinancialRecord.java
│       │   │   └── User.java
│       │
│       │   ├── repository/
│       │   │   ├── FinancialRecordRepository.java
│       │   │   └── UserRepository.java
│       │
│       │   ├── security/
│       │   │   ├── CustomUserDetails.java
│       │   │   ├── CustomUserDetailsService.java
│       │   │   ├── JwtAuthenticationFilter.java
│       │   │   ├── JwtTokenProvider.java
│       │   │   └── UserSecurity.java
│       │
│       │   └── service/
│       │       ├── AuthService.java
│       │       ├── DashboardService.java
│       │       ├── FinancialRecordService.java
│       │       └── UserService.java
│
│       └── resources/
│           └── application.properties

Getting Started
 Prerequisites
Java 17 or higher
MySQL 8.0 or higher
Springboot 3.2.5
Maven 3.6 or higher
Installation Steps
1. Create Database
CREATE DATABASE finacedashboardsystem;
EXIT;
2. Configure Database
Edit `src/main/resources/application.properties`:

Properties:
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql:///finacedashboardsystem
spring.datasource.username=root
spring.datasource.password=root
spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
spring.jpa.properties.hibernate.format_sql=true
3. Configure JWT Secret
jwt.secret=a3f5c8d2e1b4a7c9f6d3e8b1a4c7f9d2e5b8a3c6f9d2e4b7a1c5f8d3e6b9a2c4f7
jwt.expiration=86400000
4. Build and Run
mvn clean install
 Run the application
mvn spring-boot:run
Expected Output:
========================================
Finance Backend Started Successfully!
http://localhost:8080
========================================
 Role-Based Access Control
Role-Based Access Table
 VIEWER
•	✅ View Dashboard 
•	❌ View Financial Records 
•	❌ Create Records 
•	❌ Update Records 
•	❌ Delete Records 
•	❌ View Users 
•	❌ Manage Users 
________________________________________
 ANALYST
•	✅ View Dashboard 
•	✅ View Financial Records (own only) 
•	✅ Create Records 
•	✅ Update Records (own only) 
•	✅ Delete Records (own only) 
•	❌ View Users 
•	❌ Manage Users 
________________________________________
 ADMIN
•	✅ View Dashboard 
•	✅ View Financial Records (all) 
•	✅ Create Records 
•	✅ Update Records (all) 
•	✅ Delete Records (all) 
•	✅ View Users 
•	✅ Manage Users

Finance Dashboard API Documentation
 Base URL
http://localhost:8080
 Authentication
All protected endpoints require:
Authorization: Bearer  token
________________________________________
 API Endpoints
 Authentication
Method	Endpoint	Description
POST	/api/auth/register	Register new user
POST	/api/auth/login	Login and get JWT token
________________________________________
 Dashboard
Method	Endpoint	Description
GET	/api/dashboard/summary	Get financial analytics data
________________________________________


 Financial Records
Method	Endpoint	Description
POST	/api/records	Create a new record
PUT	/api/records/{id}	Update existing record
DELETE	/api/records/{id}	Delete record
GET	/api/records/user/{userId}	Get records by user
GET	/api/records	Get all records (Admin only)
________________________________________
 User Management
Method	Endpoint	Description
GET	/api/users	Get all users (Admin)
GET	/api/users/{id}	Get user by ID
PUT	/api/users/{id}/role	Update user role (Admin)
PUT	/api/users/{id}/status	Update user status (Admin)
DELETE	/api/users/{id}	Delete user (Admin)
________________________________________
 API Examples
 1. Register User
Request
POST http://localhost:8080/api/auth/register \
"Content-Type: application/json" \
'{
  "username": "john_analyst",
  "email": "john@example.com",
  "password": "password123",
  "role": "ANALYST"
}'
Response
{
  "id": 1,
  "username": "john_analyst",
  "email": "john@example.com",
  "role": "ANALYST",
  "status": "ACTIVE",
  "message": "User registered successfully"
}
________________________________________
 2. Login
Request
 POST http://localhost:8080/api/auth/login \
 "Content-Type: application/json" \
'{
  "username": "john_analyst",
  "password": "password123"
}'
Response
{
  "token": "eyJhbGciOiJIUzUxMiJ9...",
  "tokenType": "Bearer",
  "userId": 1,
  "username": "john_analyst",
  "email": "john@example.com",
  "role": "ANALYST",
  "message": "Login successful"
}
________________________________________
3. Create Financial Record
Request
 POST http://localhost:8080/api/records \
 "Content-Type: application/json" \
 "Authorization: Bearer YOUR_TOKEN" \
-d '{
  "amount": 5000.00,
  "type": "INCOME",
  "category": "Salary",
  "date": "2024-01-15",
  "description": "January salary"
}'
Response
{
  "id": 1,
  "amount": 5000.00,
  "type": "INCOME",
  "category": "Salary",
  "date": "2024-01-15",
  "description": "January salary",
  "userId": 1,
  "username": "john_analyst"
}
________________________________________
4. Dashboard Summary
Response
{
  "totalIncome": 5000.00,
  "totalExpenses": 1700.00,
  "netBalance": 3300.00,
  "totalTransactions": 4,
  "incomeByCategory": {
    "Salary": 5000.00
  },
  "expenseByCategory": {
    "Rent": 1200.00,
    "Groceries": 350.00,
    "Utilities": 150.00
  },
  "monthlyTrend": {
    "incomeTrend": {
      "2024-01": 5000.00,
      "2023-12": 4800.00
    },
    "expenseTrend": {
      "2024-01": 1700.00,
      "2023-12": 1650.00
    }
  }
}
________________________________________
Key Features
• JWT Authentication and Authorization
• Role-Based Access Control (Viewer / Analyst / Admin)
• Financial Analytics Dashboard
• CRUD Operations for Financial Records
• Scalable and Clean Architecture
________________________________________
Security Features
•	Password Security
Passwords are stored securely using BCrypt hashing with salt, which helps protect them from unauthorized access. 
•	JWT Authentication
The system uses JWT tokens for stateless authentication, with token expiration to maintain security. 
•	Role-Based Access Control
Method-level security using @PreAuthorize ensures users can only perform actions based on their assigned roles. 
•	Data Ownership Protection
Users are allowed to access and modify only their own data unless they have admin privileges. 
•	SQL Injection Protection
Parameterized queries through JPA and Hibernate help prevent SQL injection attacks. 
•	Input Validation
Validation annotations are used to ensure that only valid and properly formatted data is accepted by the system.
json
{
    "status": 400,
    "message": "Validation failed",
    "timestamp": "2024-01-15T10:30:00",
    "errors": {
        "amount": "Amount must be positive",
        "category": "Category is required"
    }
}`
  Performance Optimizations
- Database indexing on frequently queried columns
- JPA lazy loading for relationships
- HikariCP connection pooling
- Stateless authentication (no session overhead)
- Custom JPQL queries for aggregates
Sample Data
•	Create Admin
POST http://localhost:8080/api/auth/register \
  '{"username":"admin","email":"admin@test.com","password":"admin123","role":"ADMIN"}'

•	Create Analyst
 http://localhost:8080/api/auth/register \
'{"username":"analyst","email":"analyst@test.com","password":"analyst123","role":"ANALYST"}'

•	Create Viewer
POST http://localhost:8080/api/auth/register \
  '{"username":"viewer","email":"viewer@test.com","password":"viewer123","role":"VIEWER"}'
```

•	Create Sample Financial Records
 Login as Analyst
 POST http://localhost:8080/api/auth/login \
   '{"username":"analyst","password":"analyst123"}' 


•	Create Income
 POST http://localhost:8080/api/records \
  "Authorization: Bearer $TOKEN" \
   '{"amount":5000,"type":"INCOME","category":"Salary","date":"2024-01-15"}'

•	Create Expense
 POST http://localhost:8080/api/records \
   "Authorization: Bearer $TOKEN" \
   '{"amount":1200,"type":"EXPENSE","category":"Rent","date":"2024-01-05"}

This README file is comprehensive, professional, and ready to use. It includes all necessary information for setup, usage, testing, and deployment of your Finance Backend System.
