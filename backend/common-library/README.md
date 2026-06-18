# Common Library

## Overview

The Common Library is a shared module used across all microservices within the CloudOffice Provisioning Platform.

Its primary purpose is to eliminate duplicate code and provide reusable components that can be consumed by multiple services.

This module is packaged as a reusable library and included as a dependency in other microservices.

---

## Objectives

- Promote code reusability.
- Maintain consistency across microservices.
- Reduce duplication.
- Standardize API responses.
- Centralize exception handling models.
- Provide common DTOs and event models.
- Define shared constants and utility classes.

---

## Technology Stack

- Java 21
- Spring Boot 3.x
- Gradle

---

## Module Information

### Module Name

common-library

### Module Type

Reusable Shared Library

### Deployment

This module is not deployed independently.

It is imported as a dependency by:

- auth-service
- user-service
- organization-service
- bundle-service
- subscription-service
- provisioning-service
- notification-service
- audit-service
- recommendation-service
- api-gateway

---

## Functional Requirements

### Standard API Response Model

Provide a common response structure for all REST APIs.

Example:

```json
{
  "success": true,
  "message": "User created successfully",
  "data": {}
}
```

---

### Common Exception Models

Provide reusable exception classes.

Examples:

- ResourceNotFoundException
- BadRequestException
- ValidationException
- UnauthorizedException
- ForbiddenException

---

### Global Error Response Structure

Provide a standard error response model.

Example:

```json
{
  "timestamp": "2026-01-01T10:00:00",
  "status": 404,
  "error": "Resource Not Found",
  "message": "User not found",
  "path": "/users/1"
}
```

---

### Common Constants

Provide centralized constants.

Examples:

- Roles
- Status values
- Header names
- Kafka topic names
- Event types

Example:

ROLE_ADMIN

ROLE_USER

STATUS_ACTIVE

STATUS_INACTIVE

---

### Common Utility Classes

Provide reusable helper methods.

Examples:

- Date utilities
- String utilities
- Validation utilities
- UUID generation utilities

---

### Pagination Models

Provide reusable pagination request and response classes.

Example:

```json
{
  "page": 0,
  "size": 10,
  "sortBy": "createdDate",
  "sortDirection": "DESC"
}
```

---

### Audit Models

Provide common audit information.

Fields:

- createdBy
- createdDate
- updatedBy
- updatedDate

These models can be extended by entities in various services.

---

### Kafka Event Models

Provide shared event classes used for inter-service communication.

Examples:

### SubscriptionCreatedEvent

Fields:

- subscriptionId
- organizationId
- bundleId
- timestamp

### ProvisioningCompletedEvent

Fields:

- requestId
- status
- timestamp

### NotificationRequestedEvent

Fields:

- recipient
- subject
- message
- timestamp

---

### Common DTOs

Provide reusable data transfer objects.

Examples:

### UserSummaryDto

Fields:

- userId
- username
- email

### OrganizationSummaryDto

Fields:

- organizationId
- organizationName

---

## Suggested Package Structure

```text
common-library
│
├── constants
│   ├── RoleConstants
│   ├── StatusConstants
│   └── KafkaTopics
│
├── dto
│   ├── ApiResponse
│   ├── UserSummaryDto
│   └── OrganizationSummaryDto
│
├── event
│   ├── SubscriptionCreatedEvent
│   ├── ProvisioningCompletedEvent
│   └── NotificationRequestedEvent
│
├── exception
│   ├── ResourceNotFoundException
│   ├── BadRequestException
│   ├── UnauthorizedException
│   └── ForbiddenException
│
├── model
│   ├── ErrorResponse
│   ├── AuditModel
│   └── PaginationRequest
│
└── util
    ├── DateUtil
    ├── StringUtil
    └── ValidationUtil
```

---

## Non-Functional Requirements

### Reusability

All classes must be generic and reusable.

### Maintainability

Changes should be backward compatible whenever possible.

### Lightweight Design

Avoid adding service-specific business logic.

### Independence

The library must not depend on any microservice.

---

## Acceptance Criteria

### Build Validation

The library builds successfully using Gradle.

### Dependency Validation

Microservices can import the library successfully.

### Reusability Validation

Common DTOs, exceptions, and constants are accessible from all services.

### Event Validation

Kafka event classes can be shared across producer and consumer services.

---

## Out of Scope

The following are not part of this module:

- REST Controllers
- Service Layer Logic
- Repository Layer
- Database Access
- Authentication Logic
- Authorization Logic
- Kafka Producers
- Kafka Consumers
- Business Rules

These responsibilities belong to individual microservices.
