# User Service

## Overview

The User Service is responsible for managing platform users within the CloudOffice Provisioning Platform.

It provides APIs for creating, updating, retrieving, and managing user information.

The service acts as the system of record for user profile data and is consumed by other microservices when user information is required.

Authentication and authorization are handled by the Auth Service and are outside the scope of this service.

---

## Objectives

- Manage user information.
- Create and maintain user profiles.
- Support user search and retrieval.
- Maintain user status.
- Provide user-related APIs for internal platform operations.
- Register with Eureka Discovery Server.

---

## Technology Stack

- Java 21
- Spring Boot 3.x
- Spring Data JPA
- MySQL
- Spring Validation
- Eureka Client
- Gradle

---

## Service Information

### Service Name

user-service

### Default Port

8081

### Database

user_db

---

## Business Context

A user represents an individual who can access and use the CloudOffice platform.

Examples:

- Platform Administrator
- Organization Administrator
- Standard User
- Support User

---

## User Lifecycle

Create User

↓

Activate User

↓

Update User

↓

Deactivate User

↓

Reactivate User (Optional)

---

## Functional Requirements

### Create User

Create a new user profile.

Required Fields:

- First Name
- Last Name
- Email
- Role

Optional Fields:

- Phone Number

Validation Rules:

- Email must be unique.
- Email must follow valid format.
- First Name cannot be empty.
- Last Name cannot be empty.

---

### Get User By ID

Retrieve a specific user.

Input:

User ID

Output:

User details

---

### Get All Users

Retrieve all users.

Support:

- Pagination
- Sorting

---

### Update User

Update existing user information.

Allowed Updates:

- First Name
- Last Name
- Phone Number
- Status

Not Allowed:

- User ID modification

---

### Deactivate User

Mark a user as inactive.

No physical deletion should occur.

Status:

INACTIVE

---

### Activate User

Activate an inactive user.

Status:

ACTIVE

---

## Database Design

### Table: users

| Column       | Type         |
| ------------ | ------------ |
| id           | BIGINT       |
| first_name   | VARCHAR(100) |
| last_name    | VARCHAR(100) |
| email        | VARCHAR(255) |
| phone_number | VARCHAR(20)  |
| role         | VARCHAR(50)  |
| status       | VARCHAR(20)  |
| created_date | TIMESTAMP    |
| updated_date | TIMESTAMP    |

---

## Entity Requirements

### User

Fields:

- id
- firstName
- lastName
- email
- phoneNumber
- role
- status
- createdDate
- updatedDate

---

## API Requirements

### Create User

POST /api/v1/users

Request

```json
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com",
  "phoneNumber": "9876543210",
  "role": "USER"
}
```

Response

```json
{
  "success": true,
  "message": "User created successfully",
  "data": {
    "id": 1
  }
}
```

---

### Get User By ID

GET /api/v1/users/{id}

Response

```json
{
  "id": 1,
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com",
  "role": "USER",
  "status": "ACTIVE"
}
```

---

### Get All Users

GET /api/v1/users

Query Parameters

- page
- size
- sort

Example

GET /api/v1/users?page=0&size=10

---

### Update User

PUT /api/v1/users/{id}

---

### Activate User

PATCH /api/v1/users/{id}/activate

---

### Deactivate User

PATCH /api/v1/users/{id}/deactivate

---

## Suggested Package Structure

```text
user-service
│
├── controller
│   └── UserController
│
├── service
│   ├── UserService
│   └── UserServiceImpl
│
├── repository
│   └── UserRepository
│
├── entity
│   └── User
│
├── dto
│   ├── CreateUserRequest
│   ├── UpdateUserRequest
│   └── UserResponse
│
├── mapper
│   └── UserMapper
│
├── exception
│   └── GlobalExceptionHandler
│
└── config
```

---

## Integration Requirements

### Discovery Server

The service must register with Eureka.

Configuration:

- Service Name: user-service
- Eureka URL: http://localhost:8761/eureka

---

### Common Library

The service must use:

- ApiResponse
- ErrorResponse
- RoleConstants
- StatusConstants
- ResourceNotFoundException
- BadRequestException

from common-library.

---

## Validation Requirements

### Email

- Required
- Valid email format
- Unique

### First Name

- Required
- Maximum 100 characters

### Last Name

- Required
- Maximum 100 characters

### Role

Allowed Values:

- ADMIN
- ORG_ADMIN
- USER
- SUPPORT

---

## Non-Functional Requirements

### Maintainability

Follow layered architecture.

### Scalability

Business logic must be isolated from controllers.

### Readability

Use DTOs instead of exposing entities directly.

### Reliability

Return meaningful error responses.

---

## Acceptance Criteria

### User Creation

Given valid data

When Create User API is called

Then a user record is stored successfully.

---

### Duplicate Email

Given an existing email

When Create User API is called

Then the request fails with validation error.

---

### User Retrieval

Given an existing user

When Get User API is called

Then user information is returned successfully.

---

### User Status Update

Given an active user

When Deactivate API is called

Then status becomes INACTIVE.

---

### Eureka Registration

When the application starts

Then it registers successfully with Discovery Server.

---

## Out of Scope

The following features are not part of User Service:

- Login
- Logout
- JWT Generation
- Authentication
- Authorization
- Password Management
- OAuth2
- Kafka Messaging
- Email Notifications

These responsibilities belong to other microservices.
