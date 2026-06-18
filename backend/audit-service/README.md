# Audit Service

## Overview

The Audit Service is responsible for capturing, storing, and managing audit records generated throughout the CloudOffice Provisioning Platform.

The service listens to business events from Kafka and maintains a centralized audit trail of important system activities.

Audit records provide traceability, troubleshooting support, compliance reporting, and operational visibility.

---

## Objectives

- Capture business events.
- Store audit history.
- Provide audit search capabilities.
- Maintain immutable audit records.
- Support compliance and reporting requirements.
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

audit-service

### Default Port

8088

### Database

audit_db

---

## Business Context

The platform generates many business events.

Examples:

- User Created
- User Updated
- Organization Created
- Package Created
- Subscription Created
- Subscription Cancelled
- Provisioning Started
- Provisioning Completed
- Provisioning Failed
- Notification Sent

All important business actions should be captured in the audit trail.

---

## Audit Workflow

Business Event

↓

Kafka Topic

↓

Audit Service

↓

Create Audit Record

↓

Store In Database

↓

Available For Search

---

## Functional Requirements

### Capture Audit Events

Store audit records from incoming Kafka events.

Each audit record should contain:

- Event Type
- Event Source
- Event Payload
- Event Timestamp

---

### Retrieve Audit Record

Retrieve a specific audit record.

---

### Search Audit Records

Search audit records using filters.

Supported Filters:

- Event Type
- Service Name
- Entity ID
- Date Range

---

### View Audit History

Retrieve historical audit records.

Support:

- Pagination
- Sorting

---

## Database Design

### Table: audit_logs

| Column          | Type         |
| --------------- | ------------ |
| id              | BIGINT       |
| event_type      | VARCHAR(100) |
| source_service  | VARCHAR(100) |
| entity_id       | VARCHAR(100) |
| payload         | TEXT         |
| event_timestamp | TIMESTAMP    |
| created_date    | TIMESTAMP    |

---

## Entity Requirements

### AuditLog

Fields:

- id
- eventType
- sourceService
- entityId
- payload
- eventTimestamp
- createdDate

---

## API Requirements

### Get Audit By ID

GET /api/v1/audits/{id}

---

### Get All Audits

GET /api/v1/audits

Example:

```text
GET /api/v1/audits?page=0&size=20
```

---

### Search Audits

GET /api/v1/audits/search

Examples:

```text
/api/v1/audits/search?eventType=SUBSCRIPTION_CREATED

/api/v1/audits/search?sourceService=subscription-service

/api/v1/audits/search?entityId=100

/api/v1/audits/search?startDate=2026-01-01&endDate=2026-01-31
```

---

### Get Audit History By Entity

GET /api/v1/audits/entity/{entityId}

---

## Suggested Package Structure

```text
audit-service
│
├── controller
│   └── AuditController
│
├── service
│   ├── AuditService
│   └── AuditServiceImpl
│
├── repository
│   └── AuditRepository
│
├── entity
│   └── AuditLog
│
├── dto
│   ├── AuditResponse
│   ├── AuditSummaryResponse
│   └── AuditSearchRequest
│
├── kafka
│   ├── SubscriptionCreatedConsumer
│   ├── ProvisioningCompletedConsumer
│   ├── ProvisioningFailedConsumer
│   └── NotificationSentConsumer
│
├── mapper
│   └── AuditMapper
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

- Service Name: audit-service
- Eureka URL: http://localhost:8761/eureka

---

### API Gateway

All APIs must be exposed through API Gateway.

---

### Common Library

Use:

- ApiResponse
- ErrorResponse
- SubscriptionCreatedEvent
- ProvisioningCompletedEvent
- NotificationRequestedEvent
- ResourceNotFoundException
- BadRequestException

---

## Kafka Integration

### Consumed Topics

subscription-created-topic

provisioning-completed-topic

provisioning-failed-topic

notification-sent-topic

---

### Captured Events

#### SubscriptionCreatedEvent

Store subscription creation history.

---

#### ProvisioningCompletedEvent

Store provisioning success history.

---

#### ProvisioningFailedEvent

Store provisioning failure history.

---

#### NotificationSentEvent

Store notification delivery history.

---

## Event Types

Allowed Values

```text
USER_CREATED
USER_UPDATED

ORGANIZATION_CREATED
ORGANIZATION_UPDATED

PACKAGE_CREATED
PACKAGE_UPDATED

SUBSCRIPTION_CREATED
SUBSCRIPTION_RENEWED
SUBSCRIPTION_CANCELLED

PROVISIONING_STARTED
PROVISIONING_COMPLETED
PROVISIONING_FAILED

NOTIFICATION_SENT
```

---

## Search Capabilities

### Search By Event Type

Example:

```text
SUBSCRIPTION_CREATED
```

---

### Search By Service

Example:

```text
subscription-service
```

---

### Search By Entity

Example:

```text
organizationId=10
```

---

### Search By Date Range

Example:

```text
01-Jan-2026 to 31-Jan-2026
```

---

## Validation Requirements

### Event Type

Required.

---

### Source Service

Required.

---

### Event Timestamp

Required.

---

### Payload

Required.

Must contain valid JSON.

---

## Non-Functional Requirements

### Scalability

Support high event volumes.

### Reliability

Audit records must never be modified after creation.

### Availability

Audit failures must not impact business workflows.

### Maintainability

Support new event types with minimal changes.

---

## Acceptance Criteria

### Event Consumption

Given a Kafka event

When the message is received

Then an audit record is stored.

---

### Audit Search

Given stored audit records

When search APIs are called

Then matching records are returned.

---

### Audit History

Given an entity ID

When history API is called

Then all related records are returned.

---

### Immutable Storage

Given an audit record

When stored

Then it cannot be modified.

---

### Eureka Registration

When the service starts

Then it appears in Discovery Server.

---

## Future Enhancements

### Elasticsearch Integration

Support advanced searching.

---

### Kibana Dashboard

Visualize audit activity.

---

### Audit Retention Policies

Archive old records.

---

### Audit Export

Export to:

- CSV
- Excel
- PDF

---

### Compliance Reporting

Generate audit reports.

---

### Event Analytics

Track platform activity trends.

---

## Out of Scope

The following are not part of Audit Service:

- Authentication
- User Management
- Organization Management
- Package Management
- Subscription Management
- Provisioning Execution
- Notification Delivery
- Recommendation Generation

These responsibilities belong to dedicated microservices.
