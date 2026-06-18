# Discovery Server

## Overview

The Discovery Server is the service registry component of the CloudOffice Provisioning Platform.

It uses Netflix Eureka to enable service registration and discovery among microservices. All backend services register themselves with the Discovery Server during startup, allowing dynamic service lookup without hardcoded hostnames or ports.

This service acts as the central registry for all microservices in the platform.

---

## Objectives

- Provide service registration and discovery.
- Allow microservices to locate each other dynamically.
- Support load balancing through service instances.
- Eliminate hardcoded service URLs.
- Serve as the foundation of the microservices architecture.

---

## Technology Stack

- Java 21
- Spring Boot 3.x
- Spring Cloud Netflix Eureka Server
- Gradle
- Docker (Future)
- Kubernetes (Future)

---

## Service Information

### Service Name

discovery-server

### Default Port

8761

### Registry URL

http://localhost:8761

---

## Functional Requirements

### Service Registry

The Discovery Server shall:

- Maintain a registry of active microservices.
- Track service health and availability.
- Remove unavailable instances automatically.
- Display registered services through the Eureka Dashboard.

### Service Discovery

Registered services shall be able to:

- Register automatically during startup.
- Discover other services through Eureka.
- Resolve service names instead of physical URLs.

---

## Non-Functional Requirements

### Availability

The Discovery Server must remain available before any dependent microservice starts.

### Scalability

The design should support future clustering and replication.

### Monitoring

The dashboard should display:

- Registered services
- Instance status
- Health information
- Metadata

---

## Dependencies

Required Spring Dependencies:

- Spring Cloud Netflix Eureka Server

---

## Application Configuration

The service should:

- Run on port 8761.
- Disable client registration.
- Disable registry fetching.
- Expose the Eureka Dashboard.

Example configuration:

server.port=8761

spring.application.name=discovery-server

eureka.client.register-with-eureka=false

eureka.client.fetch-registry=false

---

## Main Components

### DiscoveryServerApplication

Responsibilities:

- Start the Spring Boot application.
- Enable Eureka Server functionality.

---

## Future Enhancements

### High Availability

Support multiple Eureka nodes.

### Security

Secure the Eureka dashboard using Spring Security.

### Monitoring

Integrate:

- Spring Boot Actuator
- Micrometer
- Prometheus
- Grafana

### Cloud Deployment

Deploy using:

- Docker
- Kubernetes
- AWS

---

## Acceptance Criteria

### Startup Validation

When the application starts:

- Spring Boot starts successfully.
- Eureka Server initializes successfully.
- Dashboard becomes accessible.

### Dashboard Validation

Access:

http://localhost:8761

Expected:

- Eureka Dashboard loads successfully.
- No registration errors are displayed.

### Service Registration Validation

When another microservice starts:

- The service registers automatically.
- The service appears in the dashboard.
- Instance status becomes UP.

---

## Out of Scope

The following are not part of this service:

- Authentication
- Authorization
- Business logic
- Database integration
- Kafka integration
- External API integrations

These concerns belong to other microservices.
