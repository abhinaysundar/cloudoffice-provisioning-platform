# Recommendation Service

## Overview

The Recommendation Service is responsible for generating package recommendations for organizations within the CloudOffice Provisioning Platform.

The service analyzes organization subscriptions, package usage, and business rules to suggest relevant packages, upgrades, and additional cloud services.

The primary goal is to improve customer experience and increase platform adoption through intelligent recommendations.

---

## Objectives

- Recommend packages to organizations.
- Suggest package upgrades.
- Suggest complementary services.
- Support future AI-driven recommendations.
- Provide recommendation APIs.
- Register with Eureka Discovery Server.

---

## Technology Stack

- Java 21
- Spring Boot 3.x
- Spring Data JPA
- MySQL
- Eureka Client
- Kafka (Future)
- Gradle

---

## Service Information

### Service Name

recommendation-service

### Default Port

8089

### Database

recommendation_db

---

## Business Context

Organizations subscribe to packages.

The Recommendation Service helps identify:

- Better packages
- Additional products
- Upgrade opportunities
- Cross-sell opportunities

Example:

Organization:

ABC Technologies

Current Package:

Starter Package

Recommendation:

Business Package

Reason:

More users and additional collaboration features.

---

## Recommendation Workflow

Organization Data

↓

Subscription Data

↓

Business Rules

↓

Recommendation Engine

↓

Recommended Packages

---

## Functional Requirements

### Generate Recommendations

Generate package recommendations for an organization.

Input:

- Organization ID

Output:

- Recommended Packages
- Recommendation Reason

---

### Get Recommendations By Organization

Retrieve recommendations generated for an organization.

---

### Refresh Recommendations

Regenerate recommendations.

---

### Recommendation History

View previous recommendations.

---

## Initial Rule-Based Recommendation Engine

### Rule 1

Current Package:

Starter Package

Recommendation:

Business Package

Reason:

Additional collaboration tools available.

---

### Rule 2

Current Package:

Business Package

Recommendation:

Enterprise Package

Reason:

Advanced productivity and analytics features.

---

### Rule 3

Subscription Near Expiration

Recommendation:

Renew Current Package

Reason:

Avoid service interruption.

---

### Rule 4

Organization Has Multiple Active Subscriptions

Recommendation:

Enterprise Package

Reason:

Consolidate subscriptions.

---

## Database Design

### Table: recommendations

| Column                | Type          |
| --------------------- | ------------- |
| id                    | BIGINT        |
| organization_id       | BIGINT        |
| package_id            | BIGINT        |
| recommendation_reason | VARCHAR(1000) |
| recommendation_score  | DECIMAL(5,2)  |
| created_date          | TIMESTAMP     |

---

## Entity Requirements

### Recommendation

Fields:

- id
- organizationId
- packageId
- recommendationReason
- recommendationScore
- createdDate

---

## API Requirements

### Generate Recommendations

POST /api/v1/recommendations/generate/{organizationId}

Response

```json
{
  "success": true,
  "message": "Recommendations generated successfully"
}
```

---

### Get Recommendations

GET /api/v1/recommendations/organization/{organizationId}

Response

```json
[
  {
    "packageId": 2,
    "packageName": "Business Package",
    "recommendationReason": "Upgrade from Starter Package",
    "recommendationScore": 85
  }
]
```

---

### Refresh Recommendations

POST /api/v1/recommendations/refresh/{organizationId}

---

### Get Recommendation By ID

GET /api/v1/recommendations/{id}

---

### Search Recommendations

GET /api/v1/recommendations/search

Examples:

```text
/api/v1/recommendations/search?organizationId=1

/api/v1/recommendations/search?packageId=2
```

---

## Suggested Package Structure

```text
recommendation-service
│
├── controller
│   └── RecommendationController
│
├── service
│   ├── RecommendationService
│   └── RecommendationServiceImpl
│
├── repository
│   └── RecommendationRepository
│
├── entity
│   └── Recommendation
│
├── dto
│   ├── RecommendationResponse
│   ├── RecommendationSummaryResponse
│   └── GenerateRecommendationRequest
│
├── engine
│   ├── RecommendationEngine
│   ├── RuleEngine
│   └── PackageRecommendationRule
│
├── client
│   ├── OrganizationClient
│   ├── SubscriptionClient
│   └── PackageClient
│
├── mapper
│   └── RecommendationMapper
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

- Service Name: recommendation-service
- Eureka URL: http://localhost:8761/eureka

---

### API Gateway

All APIs must be exposed through API Gateway.

---

### Organization Service

Retrieve organization information.

Example:

GET /api/v1/organizations/{id}

---

### Subscription Service

Retrieve active subscriptions.

Example:

GET /api/v1/subscriptions/search?organizationId=1

---

### Package Service

Retrieve package details.

Example:

GET /api/v1/packages/{id}

---

### Common Library

Use:

- ApiResponse
- ErrorResponse
- ResourceNotFoundException
- BadRequestException

---

## Recommendation Scoring

### Score Range

```text
0 - 100
```

Examples:

| Score    | Meaning            |
| -------- | ------------------ |
| 90-100   | Highly Recommended |
| 70-89    | Recommended        |
| 50-69    | Optional           |
| Below 50 | Low Priority       |

---

## Validation Requirements

### Organization ID

- Required
- Must exist

---

### Package ID

- Must exist

---

### Recommendation Score

Allowed Range:

```text
0 - 100
```

---

## Non-Functional Requirements

### Scalability

Support recommendation generation for many organizations.

### Maintainability

Rules should be easy to add and modify.

### Extensibility

Allow future AI/ML integrations.

### Reliability

Recommendations should never affect core business operations.

---

## Acceptance Criteria

### Recommendation Generation

Given an organization with subscriptions

When recommendation generation is executed

Then relevant recommendations are created.

---

### Recommendation Retrieval

Given recommendations exist

When retrieval API is called

Then recommendations are returned.

---

### Rule Evaluation

Given a matching rule

When engine executes

Then recommendation is generated.

---

### Eureka Registration

When the service starts

Then it appears in Discovery Server.

---

## Kafka Integration (Future Phase)

### Consumed Topics

subscription-created-topic

subscription-renewed-topic

provisioning-completed-topic

---

### Purpose

Automatically regenerate recommendations when customer activity changes.

---

## Future Enhancements

### AI Recommendations

Integrate:

- OpenAI
- LangChain
- Spring AI

---

### Usage-Based Recommendations

Analyze product utilization.

---

### Personalized Recommendations

Recommend based on organization behavior.

---

### Recommendation Dashboard

Provide analytics and trends.

---

### Cross-Sell Engine

Examples:

- Microsoft 365 → Power BI
- Teams → SharePoint
- Google Workspace → Gemini AI

---

### Machine Learning Models

Support predictive recommendations.

---

## Out of Scope

The following are not part of Recommendation Service:

- Authentication
- User Management
- Organization Management
- Package Management
- Subscription Management
- Provisioning Execution
- Notification Delivery
- Audit Logging

These responsibilities belong to dedicated microservices.
