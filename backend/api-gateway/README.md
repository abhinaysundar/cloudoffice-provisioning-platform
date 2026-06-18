# API Gateway

## Overview

The API Gateway serves as the single entry point for all client requests in the CloudOffice Provisioning Platform.

It is responsible for routing requests to the appropriate backend microservices, enforcing security policies, validating JWT tokens, and handling common cross-cutting concerns.

All external consumers, including web and mobile applications, must communicate through the API Gateway.

---

## Objectives

- Provide a single entry point for all APIs.
- Route requests to backend microservices.
- Integrate with Eureka Service Discovery.
- Validate JWT tokens.
- Enforce security policies.
- Centralize request logging.
- Support future rate limiting and monitoring.

---

## Technology Stack

- Java 21
- Spring Boot 3.x
- Spring Cloud Gateway
- Spring Security
- Eureka Client
- Gradle

---

## Service Information

### Service Name

api-gateway

### Default Port

8080

### Gateway URL

http://localhost:8080

---

## Architecture

```text
React Application
        |
        v
   API Gateway
        |
        +--------------------+
        |                    |
        v                    v

Auth Service          User Service
        |
        +--------------------+
        |
        v

Other Microservices
```

---

## Responsibilities

### Request Routing

Route incoming requests to the correct service.

Examples:

| Request Path               | Target Service       |
| -------------------------- | -------------------- |
| /api/v1/auth/\*\*          | auth-service         |
| /api/v1/users/\*\*         | user-service         |
| /api/v1/organizations/\*\* | organization-service |
| /api/v1/bundles/\*\*       | bundle-service       |
| /api/v1/subscriptions/\*\* | subscription-service |

---

### Service Discovery

Use Eureka for dynamic service lookup.

No hardcoded service URLs should be used.

Gateway should communicate using:

```text
lb://SERVICE-NAME
```

Example:

```text
lb://USER-SERVICE
```

---

### JWT Validation

Protected APIs must require a valid JWT token.

The gateway should:

- Extract Authorization header.
- Validate token.
- Reject invalid requests.
- Forward valid requests.

---

### Request Logging

Log:

- Request URI
- HTTP Method
- Response Status
- Processing Time

---

### Error Handling

Provide standardized error responses.

Example:

```json
{
  "timestamp": "2026-01-01T10:00:00",
  "status": 401,
  "error": "Unauthorized",
  "message": "Invalid JWT token"
}
```

---

## Functional Requirements

### Route Authentication Requests

Route:

```text
/api/v1/auth/**
```

To:

```text
auth-service
```

Authentication endpoints should be publicly accessible.

Examples:

- Login
- Register
- Token Validation

---

### Route User Requests

Route:

```text
/api/v1/users/**
```

To:

```text
user-service
```

Requires authentication.

---

### Route Organization Requests

Route:

```text
/api/v1/organizations/**
```

To:

```text
organization-service
```

Requires authentication.

---

### Route Bundle Requests

Route:

```text
/api/v1/bundles/**
```

To:

```text
bundle-service
```

Requires authentication.

---

### Route Subscription Requests

Route:

```text
/api/v1/subscriptions/**
```

To:

```text
subscription-service
```

Requires authentication.

---

## Suggested Package Structure

```text
api-gateway
│
├── config
│   └── GatewayConfig
│
├── filter
│   ├── JwtAuthenticationFilter
│   ├── LoggingFilter
│   └── ErrorHandlingFilter
│
├── security
│   ├── JwtUtil
│   └── SecurityConfig
│
└── exception
    └── GatewayExceptionHandler
```

---

## Route Configuration Requirements

### Auth Service

```text
/api/v1/auth/**
```

↓

```text
auth-service
```

---

### User Service

```text
/api/v1/users/**
```

↓

```text
user-service
```

---

### Organization Service

```text
/api/v1/organizations/**
```

↓

```text
organization-service
```

---

### Bundle Service

```text
/api/v1/bundles/**
```

↓

```text
bundle-service
```

---

### Subscription Service

```text
/api/v1/subscriptions/**
```

↓

```text
subscription-service
```

---

## Security Requirements

### Public Endpoints

No JWT required.

Examples:

```text
/api/v1/auth/login
/api/v1/auth/register
/api/v1/auth/validate
```

---

### Protected Endpoints

JWT required.

Examples:

```text
/api/v1/users/**
/api/v1/organizations/**
/api/v1/bundles/**
/api/v1/subscriptions/**
```

---

### Authorization Header

Expected format:

```text
Authorization: Bearer <token>
```

---

### Invalid Token Handling

Return:

```http
401 Unauthorized
```

---

## Integration Requirements

### Discovery Server

Register with Eureka.

Configuration:

- Service Name: api-gateway
- Eureka URL: http://localhost:8761/eureka

---

### Auth Service

Use Auth Service for:

- Token validation
- User authentication

---

### Common Library

Use:

- ApiResponse
- ErrorResponse
- Common constants

where applicable.

---

## Non-Functional Requirements

### Scalability

Gateway must support multiple backend services.

### Maintainability

Routing configuration should be easy to extend.

### Security

All protected APIs must be validated before forwarding.

### Observability

Support future integration with:

- Spring Boot Actuator
- Prometheus
- Grafana

---

## Acceptance Criteria

### Service Registration

When API Gateway starts

Then it registers successfully with Eureka.

---

### Route Resolution

When a request is received

Then it is forwarded to the correct service.

---

### JWT Validation

Given a valid token

When a protected API is called

Then the request is forwarded.

---

### Invalid Token

Given an invalid token

When a protected API is called

Then HTTP 401 is returned.

---

### Public Endpoint Access

Given no token

When Login API is called

Then access is allowed.

---

## Future Enhancements

### Rate Limiting

Prevent API abuse.

Examples:

- 100 requests per minute
- 1000 requests per hour

---

### Circuit Breaker

Integrate:

- Resilience4j

---

### Distributed Tracing

Integrate:

- Zipkin
- OpenTelemetry

---

### API Documentation Aggregation

Expose centralized Swagger UI.

---

## Out of Scope

The following are not part of API Gateway:

- User Management
- Authentication Credential Storage
- Organization Management
- Subscription Management
- Bundle Management
- Kafka Processing
- Business Logic

These responsibilities belong to backend microservices.
