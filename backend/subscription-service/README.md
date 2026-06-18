# Subscription Service

## Overview

The Subscription Service is responsible for managing organization subscriptions within the CloudOffice Provisioning Platform.

A subscription represents an agreement between an organization and a package offered by the platform.

The service manages subscription creation, activation, suspension, cancellation, renewal, and lifecycle tracking.

It acts as the central business service connecting organizations and packages.

---

## Objectives

* Create subscriptions for organizations.
* Associate organizations with packages.
* Manage subscription lifecycle.
* Track subscription status.
* Publish subscription events for downstream services.
* Support provisioning workflows.
* Register with Eureka Discovery Server.

---

## Technology Stack

* Java 21
* Spring Boot 3.x
* Spring Data JPA
* MySQL
* Eureka Client
* Validation
* Kafka (Future)
* Gradle

---

## Service Information

### Service Name

subscription-service

### Default Port

8085

### Database

subscription_db

---

## Business Context

A subscription represents a customer's purchase of a package.

Examples:

Organization:

ABC Technologies

Package:

Business Package

Subscription:

ABC Technologies → Business Package

Status:

ACTIVE

---

## Subscription Lifecycle

Create Subscription

↓

Pending Activation

↓

Active

↓

Renewed

↓

Suspended

↓

Cancelled

↓

Expired

---

## Functional Requirements

### Create Subscription

Create a new subscription.

Required Fields:

* Organization ID
* Package ID
* Start Date

Validation Rules:

* Organization must exist.
* Package must exist.
* Only active organizations can subscribe.
* Only active packages can be subscribed to.

---

### Get Subscription By ID

Retrieve subscription information.

---

### Get All Subscriptions

Retrieve subscriptions.

Support:

* Pagination
* Sorting
* Filtering

---

### Activate Subscription

Activate a pending subscription.

Status:

ACTIVE

---

### Suspend Subscription

Temporarily suspend access.

Status:

SUSPENDED

---

### Cancel Subscription

Cancel a subscription.

Status:

CANCELLED

---

### Renew Subscription

Extend subscription validity.

---

### Search Subscriptions

Search by:

* Organization
* Package
* Status
* Date Range

---

## Database Design

### Table: subscriptions

| Column          | Type        |
| --------------- | ----------- |
| id              | BIGINT      |
| organization_id | BIGINT      |
| package_id      | BIGINT      |
| start_date      | DATE        |
| end_date        | DATE        |
| status          | VARCHAR(30) |
| created_date    | TIMESTAMP   |
| updated_date    | TIMESTAMP   |

---

## Entity Requirements

### Subscription

Fields:

* id
* organizationId
* packageId
* startDate
* endDate
* status
* createdDate
* updatedDate

---

## API Requirements

### Create Subscription

POST /api/v1/subscriptions

Request

```json
{
  "organizationId": 1,
  "packageId": 1,
  "startDate": "2026-01-01",
  "endDate": "2027-01-01"
}
```

Response

```json
{
  "success": true,
  "message": "Subscription created successfully",
  "data": {
    "subscriptionId": 1
  }
}
```

---

### Get Subscription By ID

GET /api/v1/subscriptions/{id}

---

### Get All Subscriptions

GET /api/v1/subscriptions

Example:

```text
GET /api/v1/subscriptions?page=0&size=10
```

---

### Activate Subscription

PATCH /api/v1/subscriptions/{id}/activate

---

### Suspend Subscription

PATCH /api/v1/subscriptions/{id}/suspend

---

### Cancel Subscription

PATCH /api/v1/subscriptions/{id}/cancel

````

---

### Renew Subscription

PATCH /api/v1/subscriptions/{id}/renew

Request

```json
{
  "newEndDate": "2028-01-01"
}
````

---

### Search Subscriptions

GET /api/v1/subscriptions/search

Examples:

```text
/api/v1/subscriptions/search?status=ACTIVE

/api/v1/subscriptions/search?organizationId=1

/api/v1/subscriptions/search?packageId=2
```

---

## Suggested Package Structure

```text
subscription-service
│
├── controller
│   └── SubscriptionController
│
├── service
│   ├── SubscriptionService
│   └── SubscriptionServiceImpl
│
├── repository
│   └── SubscriptionRepository
│
├── entity
│   └── Subscription
│
├── dto
│   ├── CreateSubscriptionRequest
│   ├── RenewSubscriptionRequest
│   ├── SubscriptionResponse
│   └── SubscriptionSummaryResponse
│
├── mapper
│   └── SubscriptionMapper
│
├── client
│   ├── OrganizationClient
│   └── PackageClient
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

* Service Name: subscription-service
* Eureka URL: http://localhost:8761/eureka

---

### API Gateway

All requests must pass through API Gateway.

---

### Organization Service

Validate organization existence.

Example:

GET /api/v1/organizations/{id}

Validation:

* Organization exists
* Organization status = ACTIVE

---

### Package Service

Validate package existence.

Example:

GET /api/v1/packages/{id}

Validation:

* Package exists
* Package status = ACTIVE

---

### Common Library

Use:

* ApiResponse
* ErrorResponse
* StatusConstants
* ResourceNotFoundException
* BadRequestException

---

## Kafka Integration (Future Phase)

### Event Publishing

When a subscription is created:

Publish:

SubscriptionCreatedEvent

Payload:

```json
{
  "subscriptionId": 1,
  "organizationId": 1,
  "packageId": 1,
  "status": "ACTIVE"
}
```

---

### Event Consumers

Future consumers:

* provisioning-service
* notification-service
* audit-service

---

## Validation Requirements

### Organization ID

* Required
* Must exist

---

### Package ID

* Required
* Must exist

---

### Start Date

* Required

---

### End Date

* Must be greater than Start Date

---

### Status

Allowed Values:

```text
PENDING
ACTIVE
SUSPENDED
CANCELLED
EXPIRED
```

---

## Non-Functional Requirements

### Maintainability

Follow layered architecture.

### Reliability

Prevent invalid subscriptions.

### Scalability

Support large subscription volumes.

### Readability

Use DTOs and mappers.

---

## Acceptance Criteria

### Create Subscription

Given a valid organization and package

When Create Subscription API is called

Then subscription is created successfully.

---

### Invalid Organization

Given a non-existing organization

When Create Subscription API is called

Then validation error is returned.

---

### Invalid Package

Given a non-existing package

When Create Subscription API is called

Then validation error is returned.

---

### Activate Subscription

Given a pending subscription

When Activate API is called

Then status becomes ACTIVE.

---

### Cancel Subscription

Given an active subscription

When Cancel API is called

Then status becomes CANCELLED.

---

### Eureka Registration

When the service starts

Then it appears in Discovery Server.

---

## Future Enhancements

### Auto Renewal

Automatically renew subscriptions.

---

### Billing Integration

Generate invoices.

---

### Payment Gateway Integration

Examples:

* Stripe
* Razorpay
* PayPal

---

### Subscription Analytics

Generate reports and dashboards.

---

### Kafka Event Streaming

Support event-driven architecture.

---

## Out of Scope

The following are not part of Subscription Service:

* Authentication
* Organization Management
* Package Management
* Provisioning Execution
* Notifications
* Audit Logging

These responsibilities belong to dedicated microservices.
