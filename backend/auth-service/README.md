# Auth Service

## Overview

The Auth Service is responsible for authentication, authorization support, and JWT token management within the CloudOffice Provisioning Platform.

It validates user credentials, generates access tokens, verifies token validity, and provides security-related functionality for the platform.

This service acts as the central authentication provider for all microservices.

---

## Objectives

- Authenticate users.
- Generate JWT access tokens.
- Validate JWT tokens.
- Manage user credentials.
- Support role-based access control.
- Integrate with User Service.
- Register with Eureka Discovery Server.

---

## Technology Stack

- Java 21
- Spring Boot 3.x
- Spring Security
- Spring Data JPA
- JWT
- MySQL
- Eureka Client
- Gradle

---

## Service Information

### Service Name

auth-service

### Default Port

8082

### Database

auth_db

---

## Business Context

The Auth Service is responsible for answering:

- Who is the user?
- Is the user authenticated?
- What roles does the user have?
- Is the token valid?

The Auth Service does not manage detailed user profile information.

---

## Authentication Flow

User Login

↓

Validate Credentials

↓

Generate JWT Token

↓

Return Access Token

↓

Client Uses Token

↓

Protected APIs Validate Token

---

## Functional Requirements

### Register User Credentials

Create authentication credentials for a user.

This operation is typically triggered after a user is created in User Service.

Required Fields:

- User ID
- Username / Email
- Password

---

### Login

Authenticate a user.

Input:

- Email
- Password

Output:

- Access Token
- Token Expiration
- User Information

---

### Validate Token

Validate JWT token integrity and expiration.

---

### Refresh Token (Future Enhancement)

Issue a new access token using a refresh token.

---

### Change Password

Allow users to update passwords.

Validation:

- Old password must match.
- New password must satisfy security rules.

---

## Database Design

### Table: auth_users

| Column       | Type         |
| ------------ | ------------ |
| id           | BIGINT       |
| user_id      | BIGINT       |
| username     | VARCHAR(255) |
| password     | VARCHAR(255) |
| enabled      | BOOLEAN      |
| created_date | TIMESTAMP    |
| updated_date | TIMESTAMP    |

---

## Entity Requirements

### AuthUser

Fields:

- id
- userId
- username
- password
- enabled
- createdDate
- updatedDate

---

## API Requirements

### Register Credentials

POST /api/v1/auth/register

Request

```json
{
  "userId": 1,
  "username": "john.doe@example.com",
  "password": "Password@123"
}
```

Response

```json
{
  "success": true,
  "message": "Credentials created successfully"
}
```

---

### Login

POST /api/v1/auth/login

Request

```json
{
  "username": "john.doe@example.com",
  "password": "Password@123"
}
```

Response

```json
{
  "accessToken": "jwt-token",
  "tokenType": "Bearer",
  "expiresIn": 3600
}
```

---

### Validate Token

POST /api/v1/auth/validate

Request

```json
{
  "token": "jwt-token"
}
```

Response

```json
{
  "valid": true
}
```

---

### Change Password

POST /api/v1/auth/change-password

Request

```json
{
  "oldPassword": "Password@123",
  "newPassword": "Password@456"
}
```

---

## Suggested Package Structure

```text
auth-service
│
├── controller
│   └── AuthController
│
├── service
│   ├── AuthService
│   └── AuthServiceImpl
│
├── repository
│   └── AuthUserRepository
│
├── entity
│   └── AuthUser
│
├── dto
│   ├── LoginRequest
│   ├── LoginResponse
│   ├── RegisterRequest
│   ├── ChangePasswordRequest
│   └── TokenValidationResponse
│
├── security
│   ├── JwtUtil
│   ├── JwtAuthenticationFilter
│   ├── CustomUserDetailsService
│   └── SecurityConfig
│
├── exception
│   └── GlobalExceptionHandler
│
└── config
```

---

## Security Requirements

### Password Encryption

Passwords must never be stored in plain text.

Use:

- BCryptPasswordEncoder

---

### JWT Token

Token must contain:

- User ID
- Username
- Roles
- Expiration Time

Example Claims:

```json
{
  "sub": "john.doe@example.com",
  "userId": 1,
  "roles": ["USER"]
}
```

---

### Public Endpoints

Accessible without authentication:

- /api/v1/auth/login
- /api/v1/auth/register

---

### Protected Endpoints

Require JWT token:

- /api/v1/auth/change-password

---

## Integration Requirements

### User Service

Auth Service must communicate with User Service to:

- Verify user existence.
- Retrieve role information.
- Validate user status.

Possible APIs:

GET /api/v1/users/{id}

GET /api/v1/users/email/{email}

---

### Discovery Server

Register with Eureka.

Configuration:

- Service Name: auth-service
- Eureka URL: http://localhost:8761/eureka

---

### Common Library

Use:

- ApiResponse
- ErrorResponse
- RoleConstants
- StatusConstants
- ResourceNotFoundException
- BadRequestException

from common-library.

---

## Validation Requirements

### Username

- Required
- Must be unique

### Password

Minimum Requirements:

- At least 8 characters
- One uppercase letter
- One lowercase letter
- One number
- One special character

Example:

Password@123

---

## Non-Functional Requirements

### Security

Passwords must be encrypted.

### Scalability

Authentication logic must remain independent from business services.

### Reliability

Invalid tokens must be rejected.

### Maintainability

Security configuration must be centralized.

---

## Acceptance Criteria

### Successful Login

Given valid credentials

When Login API is called

Then JWT token is returned.

---

### Invalid Credentials

Given incorrect password

When Login API is called

Then authentication fails.

---

### Password Encryption

Given a new user registration

When credentials are stored

Then password is encrypted using BCrypt.

---

### Token Validation

Given a valid JWT

When Validate Token API is called

Then token is accepted.

---

### Eureka Registration

When the service starts

Then it appears in Discovery Server.

---

## Future Enhancements

### Refresh Tokens

Support long-lived sessions.

### OAuth2

Support external identity providers.

Examples:

- Google
- Microsoft Azure AD
- GitHub

### Multi-Factor Authentication

Support OTP verification.

### Account Locking

Lock accounts after repeated failed login attempts.

---

## Out of Scope

The following are not part of Auth Service:

- User Profile Management
- Organization Management
- Subscription Management
- Bundle Management
- Provisioning Operations
- Kafka Messaging
- Notifications

These responsibilities belong to other microservices.
