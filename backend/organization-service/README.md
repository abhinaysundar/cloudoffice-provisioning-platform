# Organization Service

## Overview

The Organization Service is responsible for managing customer organizations within the CloudOffice Provisioning Platform.

An organization represents a customer company that subscribes to cloud products and services offered through the platform.

This service maintains organization information, lifecycle management, and organization status.

---

## Objectives

- Create and manage organizations.
- Maintain organization profile information.
- Track organization status.
- Provide organization data to other microservices.
- Support future subscription and provisioning workflows.
- Register with Eureka Discovery Server.

---

## Technology Stack

- Java 21
- Spring Boot 3.x
- Spring Data JPA
- MySQL
- Eureka Client
- Validation
- Gradle

---

## Service Information

### Service Name

organization-service

### Default Port

8083

### Database

organization_db

---

## Business Context

An organization represents a customer company using the platform.

Examples:

- ABC Technologies Pvt Ltd
- Acme Corporation
- Global Solutions Ltd
- CloudSoft Inc

Organizations purchase subscriptions and bundles through the platform.

Users are associated with organizations.

---

## Organization Lifecycle

Create Organization

↓

Activate Organization

↓

Update Organization

↓

Purchase Subscription

↓

Provision Services

↓

Deactivate Organization

---

## Functional Requirements

### Create Organization

Create a new organization.

Required Fields:

- Organization Name
- Domain Name
- Contact Email

Optional Fields:

- Contact Number
- Address
- Website

Validation Rules:

- Organization Name must be unique.
- Domain Name must be unique.
- Contact Email must be valid.

---

### Get Organization By ID

Retrieve organization details.

Input:

Organization ID

Output:

Organization information

---

### Get All Organizations

Retrieve organizations.

Support:

- Pagination
- Sorting
- Filtering

---

### Update Organization

Update organization details.

Allowed Updates:

- Organization Name
- Contact Email
- Contact Number
- Address
- Website

---

### Activate Organization

Set organization status to ACTIVE.

---

### Deactivate Organization

Set organization status to INACTIVE.

No physical deletion should occur.

---

### Search Organizations

Search organizations by:

- Name
- Domain
- Status

---

## Database Design

### Table: organizations

| Column            | Type         |
| ----------------- | ------------ |
| id                | BIGINT       |
| organization_name | VARCHAR(255) |
| domain_name       | VARCHAR(255) |
| contact_email     | VARCHAR(255) |
| contact_number    | VARCHAR(50)  |
| website           | VARCHAR(255) |
| address           | VARCHAR(500) |
| status            | VARCHAR(20)  |
| created_date      | TIMESTAMP    |
| updated_date      | TIMESTAMP    |

---

## Entity Requirements

### Organization

Fields:

- id
- organizationName
- domainName
- contactEmail
- contactNumber
- website
- address
- status
- createdDate
- updatedDate

---

## API Requirements

### Create Organization

POST /api/v1/organizations

Request

```json
{
  "organizationName": "ABC Technologies",
  "domainName": "abctech.com",
  "contactEmail": "admin@abctech.com",
  "contactNumber": "9876543210",
  "website": "https://abctech.com",
  "address": "Chennai, India"
}
```

Response

```json
{
  "success": true,
  "message": "Organization created successfully",
  "data": {
    "organizationId": 1
  }
}
```

---

### Get Organization By ID

GET /api/v1/organizations/{id}

---

### Get All Organizations

GET /api/v1/organizations

Example:

```text
GET /api/v1/organizations?page=0&size=10
```

---

### Update Organization

PUT /api/v1/organizations/{id}

---

### Activate Organization

PATCH /api/v1/organizations/{id}/activate

---

### Deactivate Organization

PATCH /api/v1/organizations/{id}/deactivate

---

### Search Organizations

GET /api/v1/organizations/search

Examples:

```text
/api/v1/organizations/search?name=abc

/api/v1/organizations/search?domain=abctech.com

/api/v1/organizations/search?status=ACTIVE
```

---

## Suggested Package Structure

```text
organization-service
│
├── controller
│   └── OrganizationController
│
├── service
│   ├── OrganizationService
│   └── OrganizationServiceImpl
│
├── repository
│   └── OrganizationRepository
│
├── entity
│   └── Organization
│
├── dto
│   ├── CreateOrganizationRequest
│   ├── UpdateOrganizationRequest
│   ├── OrganizationResponse
│   └── OrganizationSummaryResponse
│
├── mapper
│   └── OrganizationMapper
│
├── exception
│   └── GlobalExceptionHandler
│
└── config
```

---

## Integration Requirements

### Discovery Server

Register with Eureka.

Configuration:

- Service Name: organization-service
- Eureka URL: http://localhost:8761/eureka

---

### API Gateway

All requests must pass through API Gateway.

Direct external access is not allowed.

---

### Auth Service

Protected APIs require authenticated users.

JWT validation is handled by API Gateway.

---

### User Service

Future integration:

Retrieve organization administrators.

Example:

```text
GET /api/v1/users?organizationId=1
```

---

### Common Library

Use:

- ApiResponse
- ErrorResponse
- StatusConstants
- ResourceNotFoundException
- BadRequestException

---

## Validation Requirements

### Organization Name

- Required
- Unique
- Maximum 255 characters

---

### Domain Name

- Required
- Unique
- Valid domain format

Examples:

```text
abctech.com
cloudsoft.io
```

---

### Contact Email

- Required
- Valid email format

---

### Status

Allowed Values:

```text
ACTIVE
INACTIVE
```

---

## Non-Functional Requirements

### Maintainability

Follow layered architecture.

### Scalability

Support large numbers of organizations.

### Reliability

Ensure uniqueness of organization names and domains.

### Readability

Use DTOs and mappers.

---

## Acceptance Criteria

### Organization Creation

Given valid organization data

When Create Organization API is called

Then organization is stored successfully.

---

### Duplicate Organization

Given an existing organization name

When Create Organization API is called

Then validation error is returned.

---

### Duplicate Domain

Given an existing domain

When Create Organization API is called

Then validation error is returned.

---

### Organization Retrieval

Given a valid organization ID

When Get Organization API is called

Then organization details are returned.

---

### Status Update

Given an active organization

When Deactivate API is called

Then status becomes INACTIVE.

---

### Eureka Registration

When the service starts

Then it appears in Discovery Server.

---

## Future Enhancements

### Organization Settings

Store tenant-specific settings.

Examples:

- Time Zone
- Locale
- Branding

---

### Multi-Tenant Support

Support tenant isolation.

---

### Organization Hierarchy

Parent and child organizations.

---

### Organization Audit Trail

Track organization changes.

---

### Organization Administrators

Assign organization-level administrators.

---

## Out of Scope

The following are not part of Organization Service:

- User Authentication
- Subscription Management
- Bundle Management
- Provisioning Logic
- Kafka Messaging
- Notifications
- Recommendation Engine

These responsibilities belong to other microservices.
