# Business Requirements Document (BRD)
## Podoh Backend Services

**Document Version:** 2.0  
**Date:** January 9, 2026  
**Product:** Podoh Platform

---

## 1. Executive Summary

### 1.1 Purpose
This document defines the business requirements for the Podoh Backend Services - a cloud-native, microservices-based API platform that provides RESTful services for content management, device management, and schedule deployment to PVM devices.

### 1.2 Product Vision
Deliver a scalable, highly-available RESTful API platform deployed on AWS infrastructure that enables centralized control of distributed Podoh display networks with automatic scaling based on demand.

### 1.3 System Architecture Overview
The system consists of four main components:
- **Podoh Web** - Frontend web application (SPA)
- **Podoh API** - Backend RESTful services (Docker containers on AWS ECS)
- **Database** - MariaDB on AWS RDS
- **Podoh Player** - Android application running on PVM monitor boards

### 1.4 Target Deployment
- AWS ECS (Elastic Container Service) for containerized services
- AWS RDS (MariaDB) for database
- AWS S3 for media file storage
- AWS CloudFront for content delivery (CDN)
- Auto-scaling based on traffic load

---

## 2. Business Objectives

| ID | Objective | Success Metric |
|----|-----------|----------------|
| BO-001 | Enable centralized content management | Single platform manages unlimited PVM devices |
| BO-002 | Support remote device deployment | 100% of devices manageable without physical access |
| BO-003 | Provide high availability | 99.99% uptime SLA |
| BO-004 | Enable elastic scalability | Auto-scale from 10 to 10,000+ devices |
| BO-005 | Minimize operational overhead | Fully managed AWS infrastructure |
| BO-006 | Support multi-region deployment | Low-latency access globally |

---

## 3. Functional Requirements

### 3.1 RESTful API Services

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-API-001 | RESTful Architecture | Must Have | REST API following OpenAPI 3.0 specification |
| FR-API-002 | JSON Data Format | Must Have | All requests/responses in JSON format |
| FR-API-003 | API Versioning | Must Have | Version prefix (e.g., /api/v1/) |
| FR-API-004 | Authentication | Must Have | JWT-based authentication |
| FR-API-005 | Rate Limiting | Should Have | Request rate limiting per client |
| FR-API-006 | API Documentation | Must Have | Swagger/OpenAPI documentation |

### 3.2 Device Management Service

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-DEV-001 | Device Registration | Must Have | Register PVM devices by unique Player ID |
| FR-DEV-002 | Device Grouping | Must Have | Organize devices into logical groups |
| FR-DEV-003 | Device Status Monitoring | Must Have | Real-time online/offline status tracking |
| FR-DEV-004 | Device Synchronization | Must Have | Push configuration to registered devices |
| FR-DEV-005 | Group Deployment | Must Have | Deploy content to device groups simultaneously |
| FR-DEV-006 | Device Heartbeat | Must Have | WebSocket/polling for status updates |
| FR-DEV-007 | Device Naming | Should Have | Custom naming for device identification |

### 3.3 Content Management Service

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-CNT-001 | Media File Storage | Must Have | Store video, image, and HTML content in S3 |
| FR-CNT-002 | File Upload | Must Have | Multipart file upload with progress |
| FR-CNT-003 | Content Organization | Must Have | Organize content by groups |
| FR-CNT-004 | Content Deletion | Must Have | Remove content from storage and devices |
| FR-CNT-005 | Content CDN | Should Have | CloudFront distribution for fast delivery |
| FR-CNT-006 | Thumbnail Generation | Should Have | Auto-generate thumbnails for preview |

### 3.4 Schedule Management Service

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-SCH-001 | Schedule CRUD | Must Have | Create, read, update, delete schedules |
| FR-SCH-002 | Schedule Deployment | Must Have | Push schedules to target devices |
| FR-SCH-003 | Schedule Versioning | Should Have | Track schedule changes and history |
| FR-SCH-004 | Multi-Schedule Support | Must Have | Support multiple schedules per device |
| FR-SCH-005 | Sub-Schedule Support | Must Have | Time-based sub-schedules within schedule |

### 3.5 User Management Service

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-USR-001 | User CRUD | Must Have | Create, read, update, delete users |
| FR-USR-002 | Role-Based Access | Must Have | Administrator/Manager/User roles |
| FR-USR-003 | JWT Authentication | Must Have | Secure token-based auth |
| FR-USR-004 | Password Hashing | Must Have | BCrypt password encryption |
| FR-USR-005 | Group Assignment | Must Have | Assign users to device groups |
| FR-USR-006 | Session Management | Must Have | Token refresh and expiration |

### 3.6 Alert Service

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-ALT-001 | Alert Content Storage | Must Have | Store emergency alert media |
| FR-ALT-002 | Instant Push | Must Have | Push alerts to devices immediately |
| FR-ALT-003 | Group Targeting | Must Have | Target alerts to specific groups |
| FR-ALT-004 | Alert Cancellation | Must Have | Stop alerts and resume normal content |
| FR-ALT-005 | WebSocket Notification | Should Have | Real-time alert push via WebSocket |

### 3.7 Reporting Service

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-RPT-001 | Usage Logging | Must Have | Track content playback duration |
| FR-RPT-002 | Device Activity Log | Should Have | Record device connection events |
| FR-RPT-003 | Report Generation | Must Have | Generate usage reports |
| FR-RPT-004 | Export Formats | Must Have | Export to Excel (XLSX), CSV |
| FR-RPT-005 | Analytics Dashboard | Should Have | Real-time analytics data |

---

## 4. Non-Functional Requirements

### 4.1 Performance

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-PERF-001 | API Response Time | < 200ms for 95th percentile |
| NFR-PERF-002 | Concurrent Connections | Support 10,000+ simultaneous |
| NFR-PERF-003 | File Upload | Support files up to 500MB |
| NFR-PERF-004 | Database Queries | < 50ms average query time |

### 4.2 Scalability

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-SCL-001 | Horizontal Scaling | Auto-scale ECS tasks based on CPU/memory |
| NFR-SCL-002 | Database Scaling | RDS read replicas for read scaling |
| NFR-SCL-003 | Storage Scaling | Unlimited S3 storage capacity |
| NFR-SCL-004 | CDN Scaling | CloudFront edge caching |

### 4.3 Availability

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-AVL-001 | Uptime SLA | 99.99% availability |
| NFR-AVL-002 | Multi-AZ | RDS Multi-AZ deployment |
| NFR-AVL-003 | Health Checks | ECS health checks with auto-recovery |
| NFR-AVL-004 | Failover | Automatic failover for RDS |

### 4.4 Security

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-SEC-001 | HTTPS | TLS 1.3 encryption for all traffic |
| NFR-SEC-002 | Authentication | JWT with RS256 signing |
| NFR-SEC-003 | Authorization | Role-based access control (RBAC) |
| NFR-SEC-004 | Data Encryption | AES-256 encryption at rest |
| NFR-SEC-005 | VPC Security | Private subnets for services/database |
| NFR-SEC-006 | WAF | AWS WAF for API protection |

### 4.5 AWS Infrastructure Requirements

| ID | Service | Specification |
|----|---------|---------------|
| NFR-AWS-001 | ECS | Fargate launch type, auto-scaling |
| NFR-AWS-002 | RDS | MariaDB 10.6+, db.r6g.large minimum |
| NFR-AWS-003 | S3 | Standard storage class, versioning enabled |
| NFR-AWS-004 | CloudFront | Global edge distribution |
| NFR-AWS-005 | ALB | Application Load Balancer with SSL |
| NFR-AWS-006 | ECR | Container image repository |
| NFR-AWS-007 | CloudWatch | Logging and monitoring |
| NFR-AWS-008 | Secrets Manager | Database credentials storage |

---

## 5. API Endpoints

### 5.1 Authentication API

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/v1/auth/login` | POST | User login, returns JWT |
| `/api/v1/auth/logout` | POST | Invalidate token |
| `/api/v1/auth/refresh` | POST | Refresh JWT token |
| `/api/v1/auth/password` | PUT | Change password |

### 5.2 Device API

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/v1/devices` | GET | List all devices |
| `/api/v1/devices` | POST | Register new device |
| `/api/v1/devices/{id}` | GET | Get device details |
| `/api/v1/devices/{id}` | PUT | Update device |
| `/api/v1/devices/{id}` | DELETE | Remove device |
| `/api/v1/devices/{id}/status` | GET | Get device status |
| `/api/v1/devices/{id}/sync` | POST | Trigger device sync |

### 5.3 Content API

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/v1/content` | GET | List all content |
| `/api/v1/content` | POST | Upload new content |
| `/api/v1/content/{id}` | GET | Get content details |
| `/api/v1/content/{id}` | DELETE | Delete content |
| `/api/v1/content/{id}/download` | GET | Download content file |

### 5.4 Schedule API

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/v1/schedules` | GET | List all schedules |
| `/api/v1/schedules` | POST | Create schedule |
| `/api/v1/schedules/{id}` | GET | Get schedule details |
| `/api/v1/schedules/{id}` | PUT | Update schedule |
| `/api/v1/schedules/{id}` | DELETE | Delete schedule |
| `/api/v1/schedules/{id}/deploy` | POST | Deploy schedule |

### 5.5 Group API

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/v1/groups` | GET | List all groups |
| `/api/v1/groups` | POST | Create group |
| `/api/v1/groups/{id}` | PUT | Update group |
| `/api/v1/groups/{id}` | DELETE | Delete group |
| `/api/v1/groups/{id}/deploy` | POST | Deploy to group |

### 5.6 User API

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/v1/users` | GET | List all users |
| `/api/v1/users` | POST | Create user |
| `/api/v1/users/{id}` | GET | Get user details |
| `/api/v1/users/{id}` | PUT | Update user |
| `/api/v1/users/{id}` | DELETE | Delete user |

### 5.7 Alert API

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/v1/alerts` | GET | List alert content |
| `/api/v1/alerts` | POST | Upload alert content |
| `/api/v1/alerts/deploy` | POST | Deploy alert |
| `/api/v1/alerts/stop` | POST | Stop alert |

### 5.8 Report API

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/v1/reports/usage` | GET | Get usage report |
| `/api/v1/reports/usage/export` | GET | Export usage to Excel |
| `/api/v1/reports/devices` | GET | Get device report |

### 5.9 Device Sync API (for Android App)

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/v1/sync/register` | POST | Device registration |
| `/api/v1/sync/heartbeat` | POST | Device heartbeat |
| `/api/v1/sync/schedule` | GET | Get current schedule |
| `/api/v1/sync/content/{id}` | GET | Download content |
| `/api/v1/sync/log` | POST | Upload usage log |

---

## 6. Database Requirements

### 6.1 MariaDB on AWS RDS

| Parameter | Specification |
|-----------|---------------|
| Engine | MariaDB 10.6+ |
| Instance | db.r6g.large (minimum) |
| Storage | 100GB+ SSD (gp3) |
| Multi-AZ | Enabled |
| Backup | Automated daily backups, 7-day retention |
| Encryption | AES-256 at rest |

### 6.2 Database Schema Overview

| Table | Description |
|-------|-------------|
| users | User accounts and credentials |
| groups | Device groups |
| devices | Registered PVM devices |
| content | Content metadata |
| schedules | Schedule configurations |
| usage_logs | Content playback logs |
| alerts | Security alert content |
| audit_logs | System audit trail |

---

## 7. User Stories

### 7.1 DevOps Engineer
> "As a DevOps engineer, I want to deploy services to AWS ECS so that they automatically scale based on demand without manual intervention."

### 7.2 System Administrator
> "As a system administrator, I want the system to maintain 99.99% uptime so that our Podoh network is always operational."

### 7.3 IT Manager
> "As an IT manager, I want to monitor service health in CloudWatch so that I can proactively address issues before they impact users."

### 7.4 Security Officer
> "As a security officer, I want all data encrypted in transit and at rest so that customer information is protected."

### 7.5 Operations Manager
> "As an operations manager, I want the system to handle traffic spikes during peak hours so that all devices receive updates reliably."

---

## 8. Constraints

| ID | Constraint | Description |
|----|------------|-------------|
| CON-001 | AWS Region | Primary deployment in us-east-1 |
| CON-002 | Container Runtime | Docker containers only |
| CON-003 | Database Engine | MariaDB only (RDS) |
| CON-004 | API Protocol | HTTPS only (no HTTP) |
| CON-005 | File Size Limit | Maximum 500MB per file upload |

---

## 9. Assumptions

1. AWS account with appropriate permissions is available
2. Domain name and SSL certificates are provisioned
3. CI/CD pipeline infrastructure exists
4. DevOps team experienced with AWS ECS/RDS
5. Network connectivity between all components is reliable

---

## 10. Dependencies

| ID | Dependency | Description |
|----|------------|-------------|
| DEP-001 | AWS ECS | Container orchestration platform |
| DEP-002 | AWS RDS | Managed MariaDB database |
| DEP-003 | AWS S3 | Object storage for media files |
| DEP-004 | AWS ALB | Application load balancing |
| DEP-005 | AWS CloudFront | CDN for content delivery |
| DEP-006 | AWS ECR | Container image registry |
| DEP-007 | Podoh Web | Frontend web application |
| DEP-008 | Podoh Player | PVM device application |

---

## 11. Acceptance Criteria

| ID | Criteria | Verification Method |
|----|----------|---------------------|
| AC-001 | API responds within 200ms | Load testing |
| AC-002 | System scales to 1000 devices | Scalability testing |
| AC-003 | 99.99% uptime achieved | Monitoring metrics |
| AC-004 | JWT authentication works | Security testing |
| AC-005 | File upload to S3 works | Functional testing |
| AC-006 | Database failover works | Failover testing |
| AC-007 | Auto-scaling triggers correctly | Load testing |

---

## 12. Glossary

| Term | Definition |
|------|------------|
| ECS | Elastic Container Service (AWS) |
| RDS | Relational Database Service (AWS) |
| S3 | Simple Storage Service (AWS) |
| ALB | Application Load Balancer |
| CDN | Content Delivery Network |
| JWT | JSON Web Token |
| RBAC | Role-Based Access Control |
| SLA | Service Level Agreement |

---

## Document Approval

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Product Owner | | | |
| Technical Lead | | | |
| DevOps Lead | | | |
| Security Lead | | | |
| AWS Architect | | | |
