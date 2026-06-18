# CloudOffice Frontend

## Overview

The CloudOffice Frontend is a web application that provides a user-friendly interface for managing organizations, packages, subscriptions, provisioning requests, notifications, recommendations, and platform administration.

The frontend communicates exclusively with the API Gateway and consumes backend microservice APIs.

The application is designed as a modern Single Page Application (SPA) using React and TypeScript.

---

## Objectives

- Provide a centralized user interface.
- Support authentication and authorization.
- Manage organizations and subscriptions.
- Monitor provisioning activities.
- View notifications and recommendations.
- Deliver a responsive and modern user experience.

---

## Technology Stack

### Core Technologies

- React 19
- TypeScript
- Vite

### UI Framework

Choose one:

- Material UI (Recommended)
- Tailwind CSS

### State Management

Choose one:

- Redux Toolkit (Recommended)
- Zustand

### HTTP Client

- Axios

### Routing

- React Router

### Form Handling

- React Hook Form

### Validation

- Zod

### Authentication

- JWT Authentication

---

## Application Information

### Application Name

cloudoffice-ui

### Default Port

5173

### Backend Endpoint

API Gateway

Example:

```text
http://localhost:8080
```

---

## User Roles

### Platform Administrator

Permissions:

- Manage Users
- Manage Organizations
- Manage Packages
- Manage Subscriptions
- View Audit Logs
- View Provisioning Requests

---

### Organization Administrator

Permissions:

- Manage Organization
- View Subscriptions
- View Notifications
- View Recommendations

---

### Standard User

Permissions:

- View Dashboard
- View Notifications

---

## Application Architecture

```text
React Application
        |
        v
     Axios
        |
        v
   API Gateway
        |
        v
Microservices
```

---

## Functional Modules

### Authentication Module

Responsibilities:

- Login
- Logout
- Token Storage
- Protected Routes
- Session Management

---

### Dashboard Module

Responsibilities:

- Summary Statistics
- Active Organizations
- Active Packages
- Active Subscriptions
- Provisioning Status

---

### User Management Module

Responsibilities:

- Create User
- Update User
- View Users
- Search Users

Backend:

user-service

---

### Organization Management Module

Responsibilities:

- Create Organization
- Update Organization
- View Organizations
- Search Organizations

Backend:

organization-service

---

### Package Management Module

Responsibilities:

- Create Package
- Update Package
- View Packages
- Search Packages

Backend:

package-service

---

### Subscription Management Module

Responsibilities:

- Create Subscription
- Activate Subscription
- Cancel Subscription
- Renew Subscription

Backend:

subscription-service

---

### Provisioning Module

Responsibilities:

- View Provisioning Requests
- View Provisioning Status
- Retry Failed Provisioning

Backend:

provisioning-service

---

### Notification Module

Responsibilities:

- View Notifications
- Mark Notification As Read

Backend:

notification-service

---

### Recommendation Module

Responsibilities:

- View Recommendations
- Refresh Recommendations

Backend:

recommendation-service

---

### Audit Module

Responsibilities:

- View Audit Logs
- Search Audit Logs

Backend:

audit-service

---

## Suggested Folder Structure

```text
src
│
├── api
│   ├── axios.ts
│   ├── authApi.ts
│   ├── userApi.ts
│   ├── organizationApi.ts
│   ├── packageApi.ts
│   ├── subscriptionApi.ts
│   ├── provisioningApi.ts
│   ├── notificationApi.ts
│   ├── recommendationApi.ts
│   └── auditApi.ts
│
├── app
│   └── store.ts
│
├── components
│   ├── layout
│   ├── common
│   ├── tables
│   ├── forms
│   └── charts
│
├── features
│   ├── auth
│   ├── users
│   ├── organizations
│   ├── packages
│   ├── subscriptions
│   ├── provisioning
│   ├── notifications
│   ├── recommendations
│   └── audits
│
├── hooks
│
├── pages
│   ├── LoginPage
│   ├── DashboardPage
│   ├── UsersPage
│   ├── OrganizationsPage
│   ├── PackagesPage
│   ├── SubscriptionsPage
│   ├── ProvisioningPage
│   ├── NotificationsPage
│   ├── RecommendationsPage
│   └── AuditPage
│
├── routes
│   ├── AppRoutes.tsx
│   └── ProtectedRoute.tsx
│
├── types
│
├── utils
│
└── main.tsx
```

---

## Routing Requirements

### Public Routes

```text
/login
```

---

### Protected Routes

```text
/dashboard

/users

/organizations

/packages

/subscriptions

/provisioning

/notifications

/recommendations

/audits
```

---

## Authentication Flow

User Login

↓

Auth Service

↓

JWT Token

↓

Store Token

↓

Access Protected Routes

↓

Send Token In Header

---

### Authorization Header

```http
Authorization: Bearer <jwt-token>
```

---

## Dashboard Requirements

Display:

### Summary Cards

- Total Users
- Total Organizations
- Total Packages
- Active Subscriptions

---

### Recent Activities

- Latest Subscriptions
- Latest Provisioning Requests
- Latest Notifications

---

### Charts (Future)

- Subscription Trends
- Provisioning Trends
- Package Popularity

---

## API Integration Requirements

### Auth APIs

```text
POST /api/v1/auth/login
```

---

### User APIs

```text
GET /api/v1/users

POST /api/v1/users
```

---

### Organization APIs

```text
GET /api/v1/organizations

POST /api/v1/organizations
```

---

### Package APIs

```text
GET /api/v1/packages

POST /api/v1/packages
```

---

### Subscription APIs

```text
GET /api/v1/subscriptions

POST /api/v1/subscriptions
```

---

### Provisioning APIs

```text
GET /api/v1/provisioning
```

---

### Notification APIs

```text
GET /api/v1/notifications
```

---

### Recommendation APIs

```text
GET /api/v1/recommendations
```

---

### Audit APIs

```text
GET /api/v1/audits
```

---

## State Management

Recommended Redux Slices:

```text
authSlice

userSlice

organizationSlice

packageSlice

subscriptionSlice

notificationSlice

recommendationSlice
```

---

## Non-Functional Requirements

### Responsiveness

Support:

- Desktop
- Tablet
- Mobile

---

### Performance

- Lazy Loading
- Code Splitting
- Efficient API Calls

---

### Security

- JWT Authentication
- Route Protection
- Token Expiration Handling

---

### Maintainability

Feature-based folder structure.

---

## Acceptance Criteria

### Login

Given valid credentials

When Login is performed

Then Dashboard is displayed.

---

### Protected Routes

Given unauthenticated user

When accessing protected pages

Then redirect to Login.

---

### CRUD Operations

Given valid input

When forms are submitted

Then backend data is updated successfully.

---

### API Communication

Given backend availability

When requests are sent

Then data is displayed correctly.

---

## Future Enhancements

### Dark Mode

User-selectable themes.

---

### Real-Time Notifications

WebSocket integration.

---

### Advanced Dashboard

Charts and analytics.

---

### Internationalization

Multiple language support.

---

### AI Assistant

Spring AI + React Chat Interface.

---

### Progressive Web App

Offline support.

---

## Out of Scope

The following are not part of the frontend:

- Business Logic
- Authentication Validation
- Database Operations
- Kafka Processing
- Provisioning Execution

These responsibilities belong to backend microservices.
