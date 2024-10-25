# Technical Specification: Bank Account and Financial Transactions Management System

## Project Objective
Develop a secure, high-performance system enabling users to manage accounts, monitor balances, and perform financial transactions, with robust infrastructure using Redis, RabbitMQ, Liquibase, and Docker.

## General Requirements
- **Programming Language**: Java (using Spring Boot framework)
- **Architecture**: Monolithic architecture
- **Database**: PostgreSQL with Liquibase for version control
- **Caching**: Redis for improved performance
- **Messaging**: RabbitMQ for asynchronous communication
- **Containerization**: Docker for environment consistency and simplified deployment
- **Authentication & Authorization**: Spring Security with JWT
- **API Documentation**: Swagger or OpenAPI

## Core Modules

### 1. User and Authentication Module
- **User Registration and Login**: New user registration with JWT authentication.
- **Authorization**: Role-based access control for different user types (admin and user roles).
- **Two-Factor Authentication (Optional)**: Extra layer of security using email/SMS verification.

### 2. Account Management Module
- **Account Creation and Viewing**: Create accounts (checking/savings) and view account details and balances.
- **Transaction History**: Logs transactions with history accessible to users.

### 3. Financial Transactions Module
- **Deposit, Withdrawal, and Transfers**:
  - **Internal Transfers**: Between bank accounts within the system.
  - **External Transfers**: To accounts in other banks.
- **Balance Verification**: Ensures sufficient funds before initiating transactions.

### 4. Fraud and Security Monitoring Module
- **Anomaly Detection**: Detects suspicious activity through predefined rules.
- **Security Alerts**: Notifies users of unusual account activity.

### 5. Notification Module
- **Transaction and Security Notifications**: Notifies users via SMS/Email about transaction and security events.

### 6. Admin Panel
- **User Management**: Monitors users, manages accounts, and has options to block/deactivate accounts.
- **Reports**: Generates reports for admins on account activities, suspicious transactions, etc.

## Technical Details
- **Monolithic Application Structure**: All modules will run within a single Spring Boot application.

### Database and Versioning
- **Liquibase**: Manages database schema migrations, ensuring consistent changes across environments.
  
### Database Tables:
- **User Table**: `user_id`, `name`, `email`, `phone`, `password`, `role`
- **Account Table**: `account_id`, `user_id`, `account_type`, `balance`, `created_at`
- **Transaction Table**: `transaction_id`, `account_id`, `transaction_type`, `amount`, `transaction_date`
- **Fraud Detection Table**: `fraud_id`, `transaction_id`, `risk_score`, `detected_at`

### Caching with Redis
- **Session and Data Caching**: Improves response times for frequently accessed data, such as account balances and transaction history.
- **Cache Configuration**: Expiration times for sensitive data to ensure accuracy.

### Messaging with RabbitMQ
- **Asynchronous Event Handling**:
  - **Notifications**: Uses RabbitMQ to send notifications (e.g., SMS/email alerts for transactions).
  - **Transaction Processing**: Queueing certain transactions to process asynchronously.

#### Event Types:
- **Transaction Events**: Triggers notifications and account updates.
- **Security Events**: Alerts users to security issues.

### Containerization with Docker
- **Docker Compose**: Creates a local environment with PostgreSQL, Redis, and RabbitMQ containers.
- **Consistent Deployment**: Docker images will allow seamless deployment across development, testing, and production environments.

### API Endpoints:
- **POST /api/accounts** - Creates new accounts.
- **GET /api/accounts/{accountId}** - Retrieves account information.
- **POST /api/transactions/deposit** - Deposits funds.
- **POST /api/transactions/withdraw** - Withdraws funds.
- **POST /api/transactions/transfer** - Transfers funds between accounts.

### Endpoints for Admin:
- **GET /api/admin/users** - Lists all users.
- **GET /api/admin/reports** - Generates and retrieves reports.

### Security
- **JWT Authentication**: Provides secure token-based access to resources.
- **Rate Limiting**: Limits repeated requests for API protection.
- **Fraud Monitoring**: Alerts on suspicious activity.

### Performance Optimization
- **Caching with Redis**: Frequently accessed data cached for faster retrieval.
- **Database Indexing**: Indexes on frequently accessed columns.
- **Batch Processing**: Optimizes processing of notifications and other frequent tasks.

## Setup and Installation
### Configuration:
- **Docker Compose**: Deploys PostgreSQL, Redis, RabbitMQ, and the Spring Boot application.
- **Configure application.properties** with database, Redis, and RabbitMQ credentials.

### Liquibase Database Migrations:
- Use Liquibase scripts to manage and version database schema changes.

### Run Instructions:
1. Start containers with Docker Compose.
2. Access Swagger documentation for API testing.

## Testing Scenarios
- **Unit and Integration Testing**: Covers CRUD operations, transaction handling, and notification triggers.
- **Load Testing**: Ensures application stability and performance with high transaction volumes.
- **Security Testing**: Verifies JWT handling, role-based access, and data protection.

## Git Flow

This project utilizes the Git Flow branching model to manage the development process. This model includes the following branches:

1. **Development Branch (`develop`)**: This branch is the main area for developing new features. Developers write code here, and all new features are merged into this branch. The `develop` branch should always be kept active as all new development occurs here.

2. **Pre-Production Branch (`pre-prod`)**: After the new changes are tested in the `develop` branch, they are merged into the `pre-prod` branch. This branch is used for final testing and quality assurance before the changes are released to production. A version of the application that closely resembles the production environment is created here.

3. **Production Branch (`prod`)**: Once testing is complete, changes from the `pre-prod` branch are merged into the `prod` branch. This branch contains the code that is currently in production and available to users. The `prod` branch holds the final releases and only final code changes are made here.

### Branch Strategy

- **Feature Development**: Developers create new feature branches from the `develop` branch. Once the feature is complete, it is merged back into the `develop` branch.

- **Testing**: Code in the `develop` branch is regularly tested. Once features are confirmed to work as expected, they are merged into the `pre-prod` branch.

- **Release**: After final testing in the `pre-prod` branch, the code is merged into the `prod` branch, making it live for users.

By following this Git Flow model, we manage our development process in a more efficient and structured way, ensuring the quality and reliability of the project.

