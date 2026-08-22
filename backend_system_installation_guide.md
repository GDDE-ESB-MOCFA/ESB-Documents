# MCFA E-Services – Backend System Installation Guide

MCFA E-Services (ESB Generic Template) v2.0.0

---

## Document version history

| Release no | Author   | Release Date | Description                                          |
| :--------- | :------- | :----------- | :--------------------------------------------------- |
| v1.0.0     | ESB Team | January 2026 | Initial release of Generic Template​ Backend System. |
| v2.0.0     | GDDE     | August 2026  | Updated for the MCFA deployment: GDDE-ESB-MOCFA repositories, NestJS gateway/registry/certificate/review services. |

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Requirements & Tech Stack](#requirements--tech-stack)
- [Services](#services)
  - [Backend Services](#backend-services)
  - [Database Setup](#database-setup)
- [Pre-requisite Configuration](#pre-requisite-configuration)
  - [External Integrations](#external-integrations)
- [Application Setup](#application-setup)
  - [Data Seeding](#data-seeding)
  - [Deployment](#deployment)
- [Development](#development)
  - [Local Development Setup](#local-development-setup)
  - [Building Services](#building-services)
- [API Documentation](#api-documentation)

---

## Overview

A comprehensive microservices-based platform for government e-services that manages end-to-end service workflows, including reviews and approvals, payments, notifications, digital certificate issuance, reporting, and role-based access control, with a strong focus on security, scalability, and auditability.

### Key Features

- **Microservices Architecture**: Modular, scalable, and independently deployable services
- **Multi-Gateway Pattern**: Separate gateways for public and backoffice access
- **Application Processing**: End-to-end application workflow management
- **Certificate Issuance**: Digital certificate generation and management
- **Payment Integration**: Bank integration for service fee payments
- **Review Workflows**: Configurable review and approval processes
- **Multi-channel Notifications**: SMS, Email, and Telegram notifications
- **Integration Ready**: Connections to Banks, Camdigikey, MOC, NRMIS, and Verify.gov.kh

---

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         Load Balancer / Ingress                  │
└────────────────┬────────────────────────────────┬────────────────┘
                 │                                │
    ┌────────────▼────────────┐      ┌────────────▼────────────┐
    │   Public Gateway        │      │  Backoffice Gateway     │
    └────────────┬────────────┘      └────────────┬────────────┘
                 │                                │
    ┌────────────┴────────────────────────────────┴────────────┐
    │                        API Layer                         │
    └────────────┬────────────────────────────────┬────────────┘
                 │                                │
    ┌────────────▼────────────┐      ┌────────────▼────────────┐
    │  Core Business Services │      │  Support Services       │
    │  - Address              │      │  - SMS                  │
    │  - Application          │      │  - Email                │
    │  - Authorization        │      │  - Telegram             │
    │  - Profile              │      │  - Report               │
    │  - Certificate          │      │  - MOC Client           │
    │  - Review               │      │                         │
    └────────────┬────────────┘      └────────────┬────────────┘
                 │                                │
    ┌────────────▼────────────────────────────────▼─────────────┐
    │                    Message Bus (Kafka)                    │
    └───────────────────────────────────────────────────────────┘
                                 │
    ┌────────────────────────────▼─────────────────────────────┐
    │              Data Layer (PostgreSQL)                     │
    └──────────────────────────────────────────────────────────┘
```

---

## Requirements & Tech Stack

### Backend Services

All backend microservices are built with the following stack:

- **Language**: Java 21
- **Framework**: Spring Boot 3.x
- **Persistence**: Spring Data JPA + Hibernate
- **Database**: PostgreSQL 15+
- **Messsaging**: Kafka + Avro Schema Registry
- **Object Mapping**: MapStruct
- **Boilerplate Reduction**: Lombok
- **API Documentation**: springdoc-openapi (Swagger UI)
- **Containerization**: Jib (Maven plugin)
- **Build Tool**: Maven 3.8+
- **Authentication**: JWT with Keystore

### Infrastructure & DevOps

- **Container Orchestration**: Kubernetes
- **Configuration Management**: Ansible
- **CI/CD**: Jenkins
- **GitOps**: ArgoCD
- **Message Broker**: Apache Kafka
- **Schema Registry**: Confluent Avro Schema Registry
- **Container Registry**: Private Docker Registry
- **Artifact Repository**: (Nexus/Artifactory)

### External Dependencies

- **Payment Gateway**: Bank integration APIs
- **Government Services**:
  - Camdigikey (Digital Signature)
  - MOC Client
  - Verify.gov.kh
  - NRMIS

---

## Services

### Backend Services

#### 1. **Public Gateway**

- API gateway for public-facing services
- Authentication and authorization
- Request routing and load balancing

**Repository**: `esb-public-gateway-nest`

#### 2. **Backoffice Gateway**

- API gateway for administrative services
- Audit logging
- Internal service routing

**Repository**: `esb-backoffice-gateway-nest`

#### 3. **Address Service**

- Provides reference data for countries, provinces, districts, communes, and villages
- RESTful APIs for address lookup and validation

**Repository**: `ESB-Address`
**Key Entities**: Country, Province, District, Commune, Village

#### 4. **Application Service**

- Handles service applications and submissions
- Document upload and management
- Application tracking and status updates
- Service management

**Repository**: `esb-registry-svc-nest` (applications, profiles and MCFA licensing are served by the registry service)
**Key Entities**: Application, Service, ServiceFee, ServiceDocument, ServiceDepartment

#### 5. **Authorization Service**

- User authentication and authorization
- Role-based access control (RBAC)
- JWT token generation and validation
- Permission management

**Repository**: `ESB-Authorization`
**Key Entities**: User, Role, Permission, Department

#### 6. **Profile Service**

- Business Profile
- License Information

**Repository**: `esb-registry-svc-nest` (merged into the registry service)

#### 7. **Certificate Service**

- Digital certificate generation based on Business Profile
- Certificate template management
- Certificate verification
- Verify.gov.kh QR Code generation for certificates

**Repository**: `esb-certificate-svc-nest`
**Key Entities**: Certificate, CertificateTemplate

#### 8. **Review Service**

- Application review workflows
- Multi-stage approval processes
- Review assignment and tracking
- Email/Telegram notifications for review stages

**Repository**: `esb-review-svc-nest`
**Key Entities**: ReviewWorkflow, ReviewWorkflowStep, ReviewEmailTemplate

#### 9. **MOC Client Service**

- Integration with Ministry of Commerce systems
- Business registration loopup based on CamDigiKey
- Data exchange and validation

**Repository**: `ESB-MOC-Client`

#### 10. **Report Service**

- Report management
- Dashboard data aggregation

**Repository**: `ESB-Report`

#### 11. **SMS Service**

- SMS notification Delivery
- Delivery status tracking
- Provider integration

**Repository**: `ESB-Sms`

#### 12. **Telegram Service**

- Telegram bot integration
- Group notifications
- Interactive notifications

**Repository**: `ESB-Telegram`

#### 13. **Email Service**

- Email notification delivery
- Email configuration

**Repository**: `ESB-Email`

### Frontend Applications

#### 1. **Public Portal**

- User-facing web application
- Service application submission
- Application tracking
- Certificate download
- Payment processing

**Repository**: `ESB-Registry-Public-Portal`

#### 2. **Backoffice Portal**

- Administrative web application
- Application review and approval
- User and role management
- Service configuration
- Reports and analytics
- System configuration

**Repository**: `ESB-Registry-Backoffice-Portal`

---

### Database Setup

**Database Creation Script**:

```sql
-- Create databases for each service
CREATE DATABASE esb-address;
CREATE DATABASE esb-application;
CREATE DATABASE esb-authorization;
CREATE DATABASE esb-profile;
CREATE DATABASE esb-certificate;
CREATE DATABASE esb-review;
CREATE DATABASE esb-payment;
CREATE DATABASE esb-report;
CREATE DATABASE esb-nrmis;
CREATE DATABASE esb-sms;
CREATE DATABASE esb-mail;
CREATE DATABASE esb-telegram;

-- Create service users
CREATE USER esb_template WITH ENCRYPTED PASSWORD 'secure_password';

-- Grant privileges
GRANT ALL PRIVILEGES ON DATABASE esb-address TO esb_template;
GRANT ALL PRIVILEGES ON DATABASE esb-application TO esb_template;
GRANT ALL PRIVILEGES ON DATABASE esb-authorization TO esb_template;
GRANT ALL PRIVILEGES ON DATABASE esb-profile TO esb_template;
GRANT ALL PRIVILEGES ON DATABASE esb-certificate TO esb_template;
GRANT ALL PRIVILEGES ON DATABASE esb-review TO esb_template;
GRANT ALL PRIVILEGES ON DATABASE esb-payment TO esb_template;
GRANT ALL PRIVILEGES ON DATABASE esb-report TO esb_template;
GRANT ALL PRIVILEGES ON DATABASE esb-nrmis TO esb_template;
GRANT ALL PRIVILEGES ON DATABASE esb-sms TO esb_template;
GRANT ALL PRIVILEGES ON DATABASE esb-mail TO esb_template;
GRANT ALL PRIVILEGES ON DATABASE esb-telegram TO esb_template;
```

---

## Pre-requisite Configuration

### External Integrations

#### 1. Camdigikey Integration

**Configuration** (add to Authorization Service):

```properties
# CamDigiKey
CAMDIGIKEY_PROTOCOLS=TLSv1.2
CAMDIGIKEY_CLIENT_ID=
CAMDIGIKEY_HMAC_KEY=
CAMDIGIKEY_AES_SECRET_KEY=
CAMDIGIKEY_AES_IV_PARAMS=
CAMDIGIKEY_CLIENT_DOMAIN=
CAMDIGIKEY_SERVER_BASED_URL=

CAMDIGIKEY_CLIENT_KEYSTORE_FILE=classpath:keystore/camdigikey_keystore.p12
CAMDIGIKEY_CLIENT_KEYSTORE_FILE_PASSWORD=
CAMDIGIKEY_CLIENT_KEYSTORE_CLIENT_KEY_ENTRY_NAME=camdigikey_tls_key
CAMDIGIKEY_CLIENT_TRUST_STORE_FILE=classpath:keystore/camdigikey_truststore.p12
CAMDIGIKEY_CLIENT_TRUST_STORE_FILE_PASSWORD=
CAMDIGIKEY_CLIENT_TRUST_STORE_TRUSTED_ROOT_CERT_ENTRY_NAME=camdx-rootca
```

**Required Steps**:

1. Register application with Camdigikey
2. Obtain API credentials
3. Integrate into application

#### 2. Bank Integration

Configure payment gateway for service fees.

**Configuration** (add to Application Service):

```sql
-- Bank Payment Gateway

-- Supported payment methods
```

**Database Seeding for Payment Configuration**:

```sql
-- Insert payment configurations

-- Insert supported banks

-- Insert default currency
```

#### 3. Telegram Bot Configuration

**Create Telegram Bot**:

1. Message @BotFather on Telegram
2. Create new bot: `/newbot`
3. Save bot token
4. Create notification groups
5. Add bot to groups
6. Get group chat IDs

**Configuration** (add to Telegram Service):

```properties
# Telegram Bot Configuration

# Notification Groups
```

**Notification Template Seeding**:

```sql
-- Insert Telegram notification templates
```

#### 4. Email Configuration

**Email Configuration** (add to Email Service):

```properties
# Email Configuration
```

#### 5. SMS Configuration

**SMS Provider Configuration** (add to SMS Service):

```properties
# SMS Provider Configuration
```

#### 6. MOC Client Configuration

```properties
# MOC Client Configuration
CAMDX_MOC_OPENAPI_BASEURL=
CAMDX_MOC_OPENAPI_CLIENT_SUBSYSTEM_CODE=
```

#### 7. Verify.gov.kh Configuration

```properties
# Verify.gov.kh Configuration
VERIFY_GOV_KH_API_URL=
VERIFY_GOV_KH_API_HEADER=
VERIFY_GOV_KH_API_DOCUMENT_CREATE=
VERIFY_GOV_KH_API_DOCUMENT_ISSUE=
VERIFY_GOV_KH_API_DOCUMENT_EXPIRE=
VERIFY_GOV_KH_API_DOCUMENT_REVOKE=
VERIFY_GOV_KH_API_DOCUMENT_STATUS=

# Configure in DB by CertificateTemplate
VERIFY_GOV_KH_API_KEY
VERIFY_GOV_KH_API_SECRET
```

#### 8. NRMIS Configuration

```properties
# NRMIS Configuration
NRMIS_API_URL=
NRMIS_SERVICE_CODE=
NRMIS_APP_KEY=
NRMIS_MASTER_KEY=
NRMIS_X_ROAD_CLIENT=

NRMIS_REFERENCE_PREFIX=ESB-
```

---

## Application Setup

### Data Seeding

All services require initial data seeding to function properly. Execute the following SQL scripts in order:

#### 1. User, Role, and Department Setup

**Authorization Service Database** (`esb_authorization`):

#### 2. Service, Service Document, Service Group/Department Setup

**Application Service Database** (`esb_application`):

#### 3. Service Fee, Invoice Template, Currency, Banks Setup

**Payment Service Database** (`esb_payment`):

#### 4. Review Workflow, Review Email Template Setup

**Review Service Database** (`esb_review`):

#### 5. Certificate Template, Verify.gov.kh API Keys Setup

**Certificate Service Database** (`esb_certificate`):

### Deployment

#### Kubernetes Deployment Configuration

Each service requires deployment manifests in a dedicated deployment repository.

**Repository Structure**:

```
ESB-Deployment/
├── backend/
│   ├── charts/
│   │   ├── address-service/
│   │   │   ├── deployment.yml
│   │   │   ├── service.yml
│   │   │   ├── configmap.yml
│   │   ├── application-service/
│   │   ├── authorization-service/
│   ├── ...
│   ├── Chart.yml
│   ├── values-dev.yml
│   ├── values-prod.yml
│   └── values.yml
├── frontend/
│   ├── charts/
│   │   ├── public-portal/
│   │   ├── backoffice-portal/
│   ├── Chart.yml
│   ├── values-dev.yml
│   ├── values-prod.yml
│   └── values.yml
```

---

## Development

### Local Development Setup

#### Prerequisites

1. **Install Java 21**

2. **Install Docker & Docker Compose**

3. **Install PostgreSQL** (or use Docker)

4. **Install Kafka & Schema Registry** (or use Docker)

5. **Install MinIO** (or use Docker)

6. **Install JsReport** (or use Docker)

#### Clone Repositories

```bash
# All repositories live under the GDDE-ESB-MOCFA organization
ORG=https://github.com/GDDE-ESB-MOCFA

# Create workspace
mkdir esb-workspace && cd esb-workspace

# Clone Development-Tool
git clone $ORG/ESB-Development-Tool

# Clone K8s Deployment repository
git clone $ORG/ESB-Deployment

# Clone NestJS backend services (gateways, registry, certificate, review)
git clone $ORG/esb-public-gateway-nest
git clone $ORG/esb-backoffice-gateway-nest
git clone $ORG/esb-registry-svc-nest
git clone $ORG/esb-certificate-svc-nest
git clone $ORG/esb-review-svc-nest

# Clone Java backend services
git clone $ORG/ESB-Address
git clone $ORG/ESB-Authorization
git clone $ORG/ESB-MOC-Client
git clone $ORG/ESB-Payment
git clone $ORG/ESB-Report
git clone $ORG/ESB-Sms
git clone $ORG/ESB-Email
git clone $ORG/ESB-Telegram
git clone $ORG/ESB-NRMIS

# Clone frontends
git clone $ORG/ESB-Registry-Public-Portal
git clone $ORG/ESB-Registry-Backoffice-Portal
```

#### Setup Local Environment

1. **Create local databases**:

2. **Configure environment variables**:

   ```bash
   # For each service
   cd ESB-Address
   cp .env-example .env
   # Edit .env with local values
   ```

3. **Run services locally**:

   ```bash
   # Option 1: Run individually
   cd ESB-Address
   ./mvnw spring-boot:run -Dspring-boot.run.profiles=dev

   # Option 2: Use Docker Compose
   docker-compose -f docker-compose.local.yml up -d
   ```

### Building Services

#### Maven Build

```bash
# Clean and package
./mvnw clean package

# Skip tests
./mvnw clean package -DskipTests

# Build specific profile
./mvnw clean package -Pdev
```

#### Docker Image Build

```bash
# Build local Docker image
./mvnw -Pdev jib:dockerBuild

# Build and push to registry
./mvnw -Pprod jib:build
```

---

## API Documentation

### Swagger UI Access

When running with `dev` profile, Swagger UI is available at:

- **Address Service**: `http://localhost:8080/swagger-ui.html`
- **Application Service**: `http://localhost:8081/swagger-ui.html`
- ... (each service on its respective port)

### OpenAPI Specification

OpenAPI 3.0 JSON specification available at:

- `http://localhost:8080/v3/api-docs`

### Example API Endpoints

```
# Address Service
GET    /api/v1/addresses/countries
GET    /api/v1/addresses/provinces
GET    /api/v1/addresses/districts/{provinceId}
GET    /api/v1/addresses/communes/{districtId}
GET    /api/v1/addresses/villages/{communeId}

# Application Service
POST   /api/v1/applications
GET    /api/v1/applications/{id}
GET    /api/v1/services
GET    /api/v1/services/{id}

---

### Branching Strategy

- `main` - Production-ready code
- `develop` - Integration branch for features
- `feature/*` - Feature development
- `bugfix/*` - Bug fixes
- `hotfix/*` - Production hotfixes
- `release/*` - Release preparation

---
```
