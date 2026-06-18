# Package Service

## Overview

The Package Service is responsible for managing cloud service packages offered through the CloudOffice Provisioning Platform.

A package represents a collection of cloud products and services that can be purchased and provisioned for customer organizations.

The service maintains package definitions, pricing information, package status, and package metadata.

---

## Objectives

- Create and manage service packages.
- Maintain package catalog information.
- Manage package pricing.
- Track package availability.
- Provide package information to subscription and provisioning services.
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

package-service

### Default Port

8084

### Database

package_db

---

## Business Context

A package represents a commercial offering that can be purchased by organizations.

Examples:

### Starter Package

Includes:

- Microsoft 365 Basic
- Exchange Online

---

### Business Package

Includes:

- Microsoft 365 Standard
- Teams
- OneDrive
- Exchange Online

---

### Enterprise Package

Includes:

- Microsoft 365 Enterprise
- Teams
- SharePoint
- Power BI
- Exchange Online

---

Organizations subscribe to packages.

Packages are later provisioned by the Provisioning Service.

---

## Package Lifecycle

Create Package

↓

Publish Package

↓

Available For Purchase

↓

Subscribed By Organization

↓

Provisioned

↓

Retired

---

## Functional Requirements

### Create Package

Create a new package.

Required Fields:

- Package Name
- Description
- Price
- Currency

Optional Fields:

- Features
- Product List

Validation Rules:

- Package Name must be unique.
- Price must be greater than zero.

---

### Get Package By ID

Retrieve package details.

Input:

Package ID

Output:

Package information

---

### Get All Packages

Retrieve available packages.

Support:

- Pagination
- Sorting
- Filtering

---

### Update Package

Update package information.

Allowed Updates:

- Description
- Price
- Features
- Status

---

### Activate Package

Make package available for purchase.

Status:

ACTIVE

---

### Deactivate Package

Make package unavailable.

Status:

INACTIVE

---

### Search Packages

Search packages by:

- Name
- Status
- Price Range

---

## Database Design

### Table: packages

| Column       | Type          |
| ------------ | ------------- |
| id           | BIGINT        |
| package_name | VARCHAR(255)  |
| description  | VARCHAR(1000) |
| price        | DECIMAL(10,2) |
| currency     | VARCHAR(10)   |
| status       | VARCHAR(20)   |
| created_date | TIMESTAMP     |
| updated_date | TIMESTAMP     |

---

## Future Database Design

### Table: package_products

| Column       | Type         |
| ------------ | ------------ |
| id           | BIGINT       |
| package_id   | BIGINT       |
| product_code | VARCHAR(100) |
| product_name | VARCHAR(255) |

Purpose:

Store products included in a package.

Examples:

- MICROSOFT_365
- TEAMS
- SHAREPOINT
- POWER_BI
- EXCHANGE_ONLINE

---

## Entity Requirements

### Package

Fields:

- id
- packageName
- description
- price
- currency
- status
- createdDate
- updatedDate

---

## API Requirements

### Create Package

POST /api/v1/packages

Request

```json
{
  "packageName": "Business Package",
  "description": "Business productivity suite",
  "price": 999.99,
  "currency": "USD"
}
```

Response

```json
{
  "success": true,
  "message": "Package created successfully",
  "data": {
    "packageId": 1
  }
}
```

---

### Get Package By ID

GET /api/v1/packages/{id}

---

### Get All Packages

GET /api/v1/packages

Example:

```text
GET /api/v1/packages?page=0&size=10
```

---

### Update Package

PUT /api/v1/packages/{id}

---

### Activate Package

PATCH /api/v1/packages/{id}/activate

---

### Deactivate Package

PATCH /api/v1/packages/{id}/deactivate

---

### Search Packages

GET /api/v1/packages/search

Examples:

```text
/api/v1/packages/search?name=business

/api/v1/packages/search?status=ACTIVE

/api/v1/packages/search?minPrice=100&maxPrice=1000
```

---

## Suggested Package Structure

```text
package-service
│
├── controller
│   └── PackageController
│
├── service
│   ├── PackageService
│   └── PackageServiceImpl
│
├── repository
│   └── PackageRepository
│
├── entity
│   └── Package
│
├── dto
│   ├── CreatePackageRequest
│   ├── UpdatePackageRequest
│   ├── PackageResponse
│   └── PackageSummaryResponse
│
├── mapper
│   └── PackageMapper
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

- Service Name: package-service
- Eureka URL: http://localhost:8761/eureka

---

### API Gateway

All requests must be routed through API Gateway.

---

### Subscription Service

Future integration:

Retrieve package information during subscription creation.

Example:

```text
GET /api/v1/packages/{id}
```

---

### Provisioning Service

Future integration:

Retrieve products associated with a package.

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

### Package Name

- Required
- Unique
- Maximum 255 characters

---

### Price

- Required
- Greater than zero

---

### Currency

Allowed Values:

```text
USD
EUR
GBP
INR
```

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

Support large package catalogs.

### Reliability

Ensure package names remain unique.

### Readability

Use DTOs and mappers.

---

## Acceptance Criteria

### Package Creation

Given valid package data

When Create Package API is called

Then package is stored successfully.

---

### Duplicate Package Name

Given an existing package name

When Create Package API is called

Then validation error is returned.

---

### Package Retrieval

Given a valid package ID

When Get Package API is called

Then package details are returned.

---

### Status Update

Given an active package

When Deactivate API is called

Then status becomes INACTIVE.

---

### Eureka Registration

When the service starts

Then it appears in Discovery Server.

---

## Future Enhancements

### Package Categories

Examples:

- Productivity
- Collaboration
- Security
- Analytics

---

### Package Versioning

Support multiple package versions.

---

### Package Features

Store detailed package features.

---

### Package Products

Manage products included in packages.

---

### Package Recommendations

Integrate with Recommendation Service.

---

## Out of Scope

The following are not part of Package Service:

- Subscription Management
- Organization Management
- Authentication
- Provisioning Logic
- Kafka Messaging
- Notifications

These responsibilities belong to other microservices.
