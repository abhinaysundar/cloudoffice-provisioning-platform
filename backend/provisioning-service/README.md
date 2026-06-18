# Provisioning Service

## Overview

The Provisioning Service is responsible for provisioning cloud resources after a subscription is created.

It listens for subscription-related events, processes provisioning requests, performs package-specific provisioning actions, tracks provisioning status, and publishes completion events.

This service acts as the execution engine of the CloudOffice Provisioning Platform.

---

## Objectives

- Process subscription provisioning requests.
- Provision services associated with purchased packages.
- Track provisioning progress.
- Maintain provisioning history.
- Publish provisioning status events.
- Support asynchronous event-driven workflows.
- Register with Eureka Discovery Server.

---

## Technology Stack

- Java 21
- Spring Boot 3.x
- Spring Data JPA
- MySQL
- Spring Kafka
- Eureka Client
- Gradle

---

## Service Information

### Service Name

provisioning-service

### Default Port

8086

### Database

provisioning_db

---

## Business Context

When an organization subscribes to a package, the platform must provision all resources included in that package.

Example:

Organization:

ABC Technologies

Package:

Business Package

Included Products:

- Microsoft 365
- Teams
- OneDrive
- Exchange Online

The Provisioning Service receives the request and performs provisioning operations.

---

## Provisioning Workflow

Subscription Created

↓

SubscriptionCreatedEvent

↓

Kafka Topic

↓

Provisioning Service

↓

Provision Package Resources

↓

Update Provisioning Status

↓

Publish ProvisioningCompletedEvent

---

## Functional Requirements

### Create Provisioning Request

Create provisioning records when a subscription is received.

Input:

- Subscription ID
- Organization ID
- Package ID

Output:

- Provisioning Request ID

---

### Process Provisioning

Provision all resources associated with a package.

For Phase 1:

Provisioning is simulated.

Example:

```java
Thread.sleep(5000);
```

Future phases will integrate external cloud providers.

---

### Get Provisioning Status

Retrieve provisioning status.

Statuses:

- PENDING
- IN_PROGRESS
- COMPLETED
- FAILED

---

### Retry Failed Provisioning

Retry failed provisioning requests.

---

### View Provisioning History

Retrieve historical provisioning records.

---

## Database Design

### Table: provisioning_requests

| Column          | Type          |
| --------------- | ------------- |
| id              | BIGINT        |
| subscription_id | BIGINT        |
| organization_id | BIGINT        |
| package_id      | BIGINT        |
| status          | VARCHAR(30)   |
| error_message   | VARCHAR(1000) |
| created_date    | TIMESTAMP     |
| updated_date    | TIMESTAMP     |

---

## Entity Requirements

### ProvisioningRequest

Fields:

- id
- subscriptionId
- organizationId
- packageId
- status
- errorMessage
- createdDate
- updatedDate

---

## API Requirements

### Get Provisioning By ID

GET /api/v1/provisioning/{id}

---

### Get All Provisioning Requests

GET /api/v1/provisioning

Example:

```text
GET /api/v1/provisioning?page=0&size=10
```

---

### Retry Provisioning

PATCH /api/v1/provisioning/{id}/retry

---

### Get Provisioning By Subscription

GET /api/v1/provisioning/subscription/{subscriptionId}

---

### Search Provisioning Requests

GET /api/v1/provisioning/search

Examples:

```text
/api/v1/provisioning/search?status=FAILED

/api/v1/provisioning/search?organizationId=1

/api/v1/provisioning/search?packageId=2
```

---

## Suggested Package Structure

```text
provisioning-service
│
├── controller
│   └── ProvisioningController
│
├── service
│   ├── ProvisioningService
│   └── ProvisioningServiceImpl
│
├── repository
│   └── ProvisioningRepository
│
├── entity
│   └── ProvisioningRequest
│
├── dto
│   ├── ProvisioningResponse
│   ├── RetryProvisioningRequest
│   └── ProvisioningSummaryResponse
│
├── kafka
│   ├── SubscriptionCreatedConsumer
│   ├── ProvisioningCompletedProducer
│   └── ProvisioningFailedProducer
│
├── processor
│   └── PackageProvisioningProcessor
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

- Service Name: provisioning-service
- Eureka URL: http://localhost:8761/eureka

---

### API Gateway

All APIs must be exposed through API Gateway.

---

### Subscription Service

Consumes:

SubscriptionCreatedEvent

Produced by:

subscription-service

---

### Package Service

Retrieve package details.

Example:

GET /api/v1/packages/{id}

Purpose:

Determine resources included in the package.

---

### Common Library

Use:

- ApiResponse
- ErrorResponse
- SubscriptionCreatedEvent
- ProvisioningCompletedEvent
- ResourceNotFoundException
- BadRequestException

---

## Kafka Integration

### Consumed Topic

subscription-created-topic

Event:

SubscriptionCreatedEvent

Example:

```json
{
  "subscriptionId": 1,
  "organizationId": 1,
  "packageId": 1,
  "status": "ACTIVE"
}
```

---

### Produced Topic

provisioning-completed-topic

Event:

ProvisioningCompletedEvent

Example:

```json
{
  "provisioningId": 100,
  "subscriptionId": 1,
  "status": "COMPLETED"
}
```

---

### Failure Topic

provisioning-failed-topic

Example:

```json
{
  "provisioningId": 100,
  "status": "FAILED",
  "errorMessage": "Provisioning timeout"
}
```

---

## Provisioning Status Values

Allowed Values:

```text
PENDING
IN_PROGRESS
COMPLETED
FAILED
```

---

## Validation Requirements

### Subscription ID

- Required
- Must exist

### Organization ID

- Required

### Package ID

- Required

---

## Non-Functional Requirements

### Scalability

Support concurrent provisioning requests.

### Reliability

Failed requests must be recoverable.

### Availability

Provisioning should continue even if notification services are unavailable.

### Maintainability

Provisioning logic must be separated from event processing.

---

## Acceptance Criteria

### Event Consumption

Given a SubscriptionCreatedEvent

When Kafka message is received

Then provisioning begins automatically.

---

### Successful Provisioning

Given valid package information

When provisioning executes

Then status becomes COMPLETED.

---

### Failed Provisioning

Given an unexpected processing error

When provisioning executes

Then status becomes FAILED.

---

### Completion Event

Given successful provisioning

When provisioning completes

Then ProvisioningCompletedEvent is published.

---

### Eureka Registration

When the service starts

Then it appears in Discovery Server.

---

## Future Enhancements

### Microsoft 365 Provisioning

Create:

- Users
- Licenses
- Mailboxes

---

### Google Workspace Provisioning

Create:

- Users
- Groups
- Workspace Licenses

---

### Slack Provisioning

Create:

- Workspace Members
- Channels

---

### Jira Provisioning

Create:

- Users
- Projects
- Groups

---

### Workflow Engine

Support configurable provisioning workflows.

---

### Scheduled Retry Framework

Automatically retry failed requests.

---

## Out of Scope

The following are not part of Provisioning Service:

- Authentication
- Organization Management
- Package Management
- Subscription Management
- Notification Delivery
- Audit Logging
- Recommendation Generation

These responsibilities belong to dedicated microservices.
