# Notification Service

## Overview

The Notification Service is responsible for delivering notifications to users and organizations within the CloudOffice Provisioning Platform.

It listens to business events from Kafka, creates notification records, and delivers messages through supported communication channels.

The service provides a centralized notification mechanism for the platform.

---

## Objectives

- Deliver event-driven notifications.
- Support multiple notification channels.
- Maintain notification history.
- Track notification delivery status.
- Consume Kafka events from business services.
- Register with Eureka Discovery Server.

---

## Technology Stack

- Java 21
- Spring Boot 3.x
- Spring Data JPA
- MySQL
- Spring Kafka
- Spring Mail
- Eureka Client
- Gradle

---

## Service Information

### Service Name

notification-service

### Default Port

8087

### Database

notification_db

---

## Business Context

Notifications inform users and organizations about important platform events.

Examples:

- Subscription Created
- Subscription Activated
- Provisioning Started
- Provisioning Completed
- Provisioning Failed
- Package Expired
- Subscription Renewed

---

## Notification Workflow

Business Event

↓

Kafka Topic

↓

Notification Service

↓

Create Notification Record

↓

Send Notification

↓

Update Delivery Status

---

## Functional Requirements

### Create Notification

Create notification records from incoming events.

Notification Types:

- EMAIL
- SMS
- IN_APP

---

### Send Email Notification

Send notification through email.

Example:

```text
Subject:
Subscription Activated

Message:
Your subscription has been activated successfully.
```

---

### Send SMS Notification

Future enhancement.

Example:

```text
Subscription activated successfully.
```

---

### Send In-App Notification

Store notification for portal display.

---

### Get Notification By ID

Retrieve notification details.

---

### Get Notifications By User

Retrieve user notifications.

---

### Mark Notification As Read

Update notification status.

---

## Database Design

### Table: notifications

| Column            | Type          |
| ----------------- | ------------- |
| id                | BIGINT        |
| user_id           | BIGINT        |
| notification_type | VARCHAR(30)   |
| subject           | VARCHAR(255)  |
| message           | VARCHAR(2000) |
| delivery_status   | VARCHAR(30)   |
| read_status       | BOOLEAN       |
| created_date      | TIMESTAMP     |
| updated_date      | TIMESTAMP     |

---

## Entity Requirements

### Notification

Fields:

- id
- userId
- notificationType
- subject
- message
- deliveryStatus
- readStatus
- createdDate
- updatedDate

---

## API Requirements

### Get Notification By ID

GET /api/v1/notifications/{id}

---

### Get Notifications By User

GET /api/v1/notifications/user/{userId}

---

### Mark Notification As Read

PATCH /api/v1/notifications/{id}/read

---

### Search Notifications

GET /api/v1/notifications/search

Examples:

```text
/api/v1/notifications/search?deliveryStatus=SENT

/api/v1/notifications/search?notificationType=EMAIL

/api/v1/notifications/search?userId=1
```

---

## Suggested Package Structure

```text
notification-service
│
├── controller
│   └── NotificationController
│
├── service
│   ├── NotificationService
│   └── NotificationServiceImpl
│
├── repository
│   └── NotificationRepository
│
├── entity
│   └── Notification
│
├── dto
│   ├── NotificationResponse
│   ├── NotificationSummaryResponse
│   └── MarkAsReadRequest
│
├── kafka
│   ├── SubscriptionCreatedConsumer
│   ├── ProvisioningCompletedConsumer
│   └── ProvisioningFailedConsumer
│
├── email
│   ├── EmailSender
│   └── EmailTemplateBuilder
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

- Service Name: notification-service
- Eureka URL: http://localhost:8761/eureka

---

### API Gateway

All APIs must be exposed through API Gateway.

---

### User Service

Retrieve recipient information.

Example:

GET /api/v1/users/{id}

Purpose:

- Email Address
- User Name

---

### Common Library

Use:

- ApiResponse
- ErrorResponse
- NotificationRequestedEvent
- ProvisioningCompletedEvent
- ResourceNotFoundException
- BadRequestException

---

## Kafka Integration

### Consumed Topics

subscription-created-topic

provisioning-completed-topic

provisioning-failed-topic

---

### Consumed Events

#### SubscriptionCreatedEvent

Purpose:

Send subscription confirmation.

---

#### ProvisioningCompletedEvent

Purpose:

Notify successful provisioning.

---

#### ProvisioningFailedEvent

Purpose:

Notify provisioning failure.

---

## Notification Types

Allowed Values:

```text
EMAIL
SMS
IN_APP
```

---

## Delivery Status Values

Allowed Values:

```text
PENDING
SENT
FAILED
```

---

## Read Status

Allowed Values:

```text
true
false
```

---

## Example Email Templates

### Subscription Created

Subject:

```text
Subscription Created Successfully
```

Message:

```text
Your subscription has been created successfully.
Provisioning will begin shortly.
```

---

### Provisioning Completed

Subject:

```text
Provisioning Completed
```

Message:

```text
Your cloud resources have been provisioned successfully.
```

---

### Provisioning Failed

Subject:

```text
Provisioning Failed
```

Message:

```text
An error occurred during provisioning.
Please contact support.
```

---

## Validation Requirements

### Notification Type

Required.

Allowed:

- EMAIL
- SMS
- IN_APP

---

### Subject

Required.

Maximum Length:

255 characters.

---

### Message

Required.

Maximum Length:

2000 characters.

---

## Non-Functional Requirements

### Scalability

Support large notification volumes.

### Reliability

Failed notifications must be recoverable.

### Availability

Notification failures must not affect business workflows.

### Maintainability

Notification channels should be extensible.

---

## Acceptance Criteria

### Event Consumption

Given a business event

When Kafka message is received

Then notification record is created.

---

### Email Delivery

Given valid recipient information

When notification is processed

Then email is sent successfully.

---

### Delivery Status Update

Given successful delivery

When processing completes

Then status becomes SENT.

---

### Failed Delivery

Given delivery error

When processing fails

Then status becomes FAILED.

---

### Eureka Registration

When the service starts

Then it appears in Discovery Server.

---

## Future Enhancements

### SMS Integration

Providers:

- Twilio
- AWS SNS

---

### Push Notifications

Support mobile applications.

---

### WhatsApp Notifications

Support business messaging.

---

### Template Management

Manage templates dynamically.

---

### Scheduled Notifications

Support delayed delivery.

---

### Notification Preferences

Allow users to choose channels.

---

## Out of Scope

The following are not part of Notification Service:

- Authentication
- User Management
- Package Management
- Subscription Management
- Provisioning Execution
- Audit Logging
- Recommendation Generation

These responsibilities belong to dedicated microservices.
