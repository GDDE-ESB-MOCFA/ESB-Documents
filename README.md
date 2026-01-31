# ESB Generic Template Overivew
Version 1.0.0

As part of accelerating the Royal Government of Cambodia’s digital transformation agenda, the launch of the E-Services for Business (ESB) Strategy 2025–2028 (https://digitaleconomy.gov.kh/media-hub/publication)

represents a major national milestone. The ESB Strategy aims to digitally enable 80% of high-priority public services by 2028, making them accessible through shared government portals and platforms. This initiative seeks to improve service accessibility, transparency, efficiency, and trust for businesses while strengthening coordination and interoperability across government institutions.

Implementing digital services at this scale presents significant challenges if approached on a service-by-service or ministry-by-ministry basis. Developing each system independently would be costly and time-consuming, and would likely result in fragmented user experiences, duplicated investments, and inconsistent technical and security standards. In practice, most public services follow similar business processes—such as application submission, review and approval, payment, notification, and certificate issuance—highlighting the need for a standardized and reusable approach.

To address these challenges and support the ESB Strategy’s ambitious targets, the ESB Generic Template System is being developed as a foundational digital public infrastructure. The Generic Template enables rapid onboarding of public services, ensures consistency in user experience and service quality, strengthens security, privacy, and regulatory compliance, and promotes interoperability and “once-only” data exchange across government systems. This approach allows ministries to focus on policy and service-specific requirements while leveraging a shared, trusted technical foundation.

------------------------------------------------------------------------

## ESB Generic Template System

The ESB Generic Template is an open-source government service framework designed as a shared codebase that ESB government partners can adopt, customize, and continuously develop to manage and operate their ministry’s e-services. It serves as a common platform that reduces duplication and accelerates the digital transformation of public services across sectors.

The platform provides a standardized yet flexible workflow foundation, including reviewing and approval workflows, payment processing, notifications, certificate generation, reporting, and access control. By offering these common building blocks out of the box, ministries are not required to rebuild core service logic from scratch for each new digital service.

As an open codebase, the Generic Template empowers government partners to retain ownership of their implementations, extend features according to policy or sector-specific needs, and integrate seamlessly with existing legacy or external systems. At the same time, it ensures consistent UI/UX, security, compliance, and interoperability across government digital services. Through reuse of common components and workflows, the ESB Generic Template significantly reduces development cost and time, accelerates service delivery, and supports the long-term sustainability and maintainability of Cambodia’s digital government ecosystem


## System Architecture 
![System Architecture](assets/system-architecture.png)
The backend system follows a microservice architecture, where each core business capability is implemented as an independent service with its own codebase, database, and deployment lifecycle. This approach enables:
- Independent scaling and deployment
- Clear ownership per service
- Fault isolation and higher system resilience
- Easier integration with legacy and external systems
- Long-term extensibility without platform lock-in
- All backend services communicate via secure APIs and are exposed through centralized gateways.

## Architecture Design Principal

![Architecture Design Principal](assets/architecture-design-principal.png) 

## System Workflow and Logical Design

![System Workflow and Logical Design](assets/generic-workflow-and-logical-design.png)

The ESB Generic Template follows a code-based, customizable service lifecycle model that covers application submission, validation, review and approval, payment, notification, certificate issuance, and reporting. In addition, it supports the full certificate lifecycle, including validity management, expiration, renewal or extension, suspension, and termination.

This logical design ensures consistent service behavior across ministries while remaining fully configurable through code and service configuration to meet sector-specific regulatory, policy, and operational requirements.

## ESB Generic Template Installation Guide 
## 1. Backend System Installation Guide

```properties
https://github.com/Techo-Startup-Center/ESB-Documents/blob/main/backend_system_installation_guide.md
```
🔗 **[Open Backend System Installation Guide](https://github.com/Techo-Startup-Center/ESB-Documents/blob/main/backend_system_installation_guide.md)**  

## 2. Backoffice Portal Installation Guide

```properties
https://github.com/Techo-Startup-Center/ESB-Documents/blob/main/backoffice_portal_installation_guide.md
```
🔗 **[Open Backoffice Portal Installation Guide](https://github.com/Techo-Startup-Center/ESB-Documents/blob/main/backoffice_portal_installation_guide.md)**  

## 3. Public Portal Installation Guide

```properties
https://github.com/Techo-Startup-Center/ESB-Documents/blob/main/public_portal_installation_guide.md
```
🔗 **[Open Public Portal Installation Guide](https://github.com/Techo-Startup-Center/ESB-Documents/blob/main/public_portal_installation_guide.md)**  

## Repository Structure Overview

## 1. Backend System Repositories

### 1.1. ESB-Public-Gateway

Acts as the central API gateway for all public-facing services. It handles authentication, authorization, request routing, rate limiting, and load balancing to ensure secure and scalable access for citizens and businesses. 

```properties
https://github.com/Techo-Startup-Center/ESB-Public-Gateway.git
```
🔗 **[Open ESB-Public-Gateway Repository](https://github.com/Techo-Startup-Center/ESB-Public-Gateway.git)**  


### 1.2. ESB-Back-Office-Gateway

Serves as the API gateway for administrative and internal systems. It manages secure internal service routing, access control, and audit logging for backoffice operations.

```properties
https://github.com/Techo-Startup-Center/ESB-Back-Office-Gateway.git
```
🔗 **[Open ESB-Back-Office-Gateway Repository](https://github.com/Techo-Startup-Center/ESB-Back-Office-Gateway.git)**  

### 1.3. ESB-Address

Provides standardized reference data for administrative addresses, including country, province, district, commune, and village. This service ensures consistent address usage and validation across all ESB services.
```properties
https://github.com/Techo-Startup-Center/ESB-Address.git
```
🔗 **[Open ESB-Address Repository](https://github.com/Techo-Startup-Center/ESB-Address.git)**  

### 1.4. ESB-Application

Manages the full lifecycle of service applications, including submission, document management, status tracking, and service configuration. It acts as the core service for handling digital service requests.
```properties
https://github.com/Techo-Startup-Center/ESB-Application.git
```
🔗 **[Open ESB-Application Repository](https://github.com/Techo-Startup-Center/ESB-Application.git)**  

### 1.5. ESB-Authorization

Handles user authentication and authorization using role-based access control (RBAC). It manages users, roles, permissions, and JWT tokens to enforce secure access across the ESB platform.
```properties
https://github.com/Techo-Startup-Center/ESB-Authorization.git
```
🔗 **[Open ESB-Authorization Repository](https://github.com/Techo-Startup-Center/ESB-Authorization.git)**  

### 1.6. ESB-Profile

Maintains business profiles and license information used across multiple services. It provides a unified source of business identity and regulatory data for the ESB ecosystem.
```properties
https://github.com/Techo-Startup-Center/ESB-Profile.git
```
🔗 **[Open ESB-Profile Repository](https://github.com/Techo-Startup-Center/ESB-Profile.git)**  

### 1.7. ESB-Certificate

Generates and manages digital certificates based on approved applications and business profiles. It supports certificate templates, QR code generation, and verification through Verify.gov.kh.
```properties
https://github.com/Techo-Startup-Center/ESB-Certificate.git
```
🔗 **[Open ESB-Certificate Repository](https://github.com/Techo-Startup-Center/ESB-Certificate.git)**  

### 1.8. ESB-Review

Implements configurable application review and approval workflows. It supports multi-stage reviews, reviewer assignment, tracking, and automated notifications throughout the review process.
```properties
https://github.com/Techo-Startup-Center/ESB-Review.git
```

🔗 **[Open ESB-Review Repository](https://github.com/Techo-Startup-Center/ESB-Review.git)**  

### 1.9. ESB-MOC-Client

Provides integration with Ministry of Commerce systems. It enables business registration lookup, identity validation via CamDigiKey, and secure data exchange with MOC platforms.
```properties
https://github.com/Techo-Startup-Center/ESB-MOC-Client.git
```

🔗 **[Open ESB-MOC-Client Repository](https://github.com/Techo-Startup-Center/ESB-MOC-Client.git)**  


### 1.10. ESB-Report

Aggregates and manages reporting data across ESB services. It supports dashboards, operational reports, and analytics for monitoring service performance and usage.
```properties
https://github.com/Techo-Startup-Center/ESB-Report.git
```
🔗 **[Open ESB-Report Repository](https://github.com/Techo-Startup-Center/ESB-Report.git)**  

### 1.11. ESB-SMS

Delivers SMS notifications for service updates and system events. It manages provider integration, message delivery, and delivery status tracking.

```properties
https://github.com/Techo-Startup-Center/ESB-Sms.git
```
🔗 **[Open ESB-SMS Repository](https://github.com/Techo-Startup-Center/ESB-Sms.git)**  

### 1.12. ESB-Telegram

Enables Telegram-based notifications and alerts. It supports bot integration, group messaging, and interactive notifications for operational and review workflows.
```properties
https://github.com/Techo-Startup-Center/ESB-Telegram.git
```
🔗 **[Open ESB-Telegram Repository](https://github.com/Techo-Startup-Center/ESB-Telegram.git)**  


### 1.13. ESB-Email

Handles email notification delivery and configuration. It manages email templates and integrates with approved email providers for reliable communication.
```properties
https://github.com/Techo-Startup-Center/ESB-Email.git
```
🔗 **[Open ESB-Email Repository](https://github.com/Techo-Startup-Center/ESB-Email.git)**  


## 2. Frontend Application Repositories

### 2.1 ESB-Registry-Public-Portal

Provides the public-facing web interface for businesses and citizens to discover services, submit applications, track status, make payments, and download digital certificates.
```properties
https://github.com/Techo-Startup-Center/ESB-Registry-Public-Portal.git
```
🔗 **[Open ESB-Registry-Public-Portal Repository](https://github.com/Techo-Startup-Center/ESB-Registry-Public-Portal.git)**  

### 2.2 ESB-Registry-Backoffice-Portal

Provides the administrative web interface for government officials to review applications, manage users and roles, configure services, and access reports and system settings.
```properties
https://github.com/Techo-Startup-Center/ESB-Registry-Backoffice-Portal.git
```
🔗 **[Open ESB-Registry-Backoffice-Portal Repository](https://github.com/Techo-Startup-Center/ESB-Registry-Backoffice-Portal.git)**  
