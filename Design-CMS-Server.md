# System Design Document
## Podoh Backend Services

**Document Version:** 2.0  
**Date:** January 9, 2026  
**Product:** Podoh Platform - Digital Signage CMS

---

## 1. Introduction

### 1.1 Purpose
This document provides the technical design specification for the Podoh Backend Services, detailing the cloud-native architecture, AWS infrastructure, API design, database schema, and integration patterns.

### 1.2 Scope
This design covers:
- AWS ECS containerized service architecture
- AWS RDS MariaDB database design
- RESTful API specifications
- Service communication patterns
- Security implementation
- Scalability and high availability

### 1.3 References
- BRD-CMS-Server.md
- AWS Well-Architected Framework

---

## 2. System Architecture

### 2.1 High-Level Architecture

```mermaid
graph TB
    subgraph "Client Layer"
        WEB[CMS Web App<br/>React SPA]
        ANDROID[Android App<br/>PVM Devices]
    end
    
    subgraph "AWS Cloud"
        subgraph "Edge Layer"
            CF[CloudFront CDN]
            WAF[AWS WAF]
        end
        
        subgraph "Load Balancing"
            ALB[Application Load Balancer]
        end
        
        subgraph "Compute - ECS Fargate"
            subgraph "API Services"
                AUTH[Auth Service]
                DEVICE[Device Service]
                CONTENT[Content Service]
                SCHEDULE[Schedule Service]
                ALERT[Alert Service]
                REPORT[Report Service]
            end
        end
        
        subgraph "Data Layer"
            RDS[(MariaDB<br/>AWS RDS)]
            S3[(S3 Bucket<br/>Media Storage)]
            REDIS[(ElastiCache<br/>Redis)]
        end
        
        subgraph "Supporting Services"
            SQS[SQS Queue]
            SNS[SNS Topics]
            CW[CloudWatch]
            SM[Secrets Manager]
        end
    end
    
    WEB --> CF
    CF --> WAF
    WAF --> ALB
    ALB --> AUTH
    ALB --> DEVICE
    ALB --> CONTENT
    ALB --> SCHEDULE
    ALB --> ALERT
    ALB --> REPORT
    
    ANDROID --> ALB
    
    AUTH --> RDS
    AUTH --> REDIS
    DEVICE --> RDS
    DEVICE --> SQS
    CONTENT --> RDS
    CONTENT --> S3
    SCHEDULE --> RDS
    ALERT --> RDS
    ALERT --> SNS
    REPORT --> RDS
    
    CF --> S3
```

### 2.2 Component Architecture

```mermaid
graph LR
    subgraph "CMS Web (Frontend)"
        SPA[React SPA]
    end
    
    subgraph "RESTful Services (Backend)"
        API[API Gateway Layer]
        SVC[Service Layer]
        REPO[Repository Layer]
    end
    
    subgraph "Database"
        DB[(MariaDB)]
    end
    
    subgraph "Storage"
        OBJ[(S3)]
    end
    
    subgraph "Android App"
        APP[DID Screen App]
    end
    
    SPA -->|HTTPS/JSON| API
    APP -->|HTTPS/JSON| API
    API --> SVC
    SVC --> REPO
    REPO --> DB
    SVC --> OBJ
```

### 2.3 Four-Tier Architecture Overview

```mermaid
flowchart TB
    subgraph Tier1["Tier 1: Presentation"]
        CMS_WEB["CMS Web Application<br/>(React SPA on CloudFront)"]
    end
    
    subgraph Tier2["Tier 2: API"]
        REST["RESTful Services<br/>(Docker on ECS Fargate)"]
    end
    
    subgraph Tier3["Tier 3: Data"]
        MARIA["MariaDB<br/>(AWS RDS Multi-AZ)"]
        S3B["Media Storage<br/>(AWS S3)"]
    end
    
    subgraph Tier4["Tier 4: Device"]
        ANDROID_APP["Android App<br/>(PVM Monitor Board)"]
    end
    
    CMS_WEB <-->|REST API| REST
    REST <-->|SQL| MARIA
    REST <-->|S3 API| S3B
    ANDROID_APP <-->|REST API| REST
    ANDROID_APP -.->|Download| S3B
```

---

## 3. AWS Infrastructure Design

### 3.1 VPC Architecture

```mermaid
graph TB
    subgraph "AWS VPC (10.0.0.0/16)"
        subgraph "Public Subnets"
            PUB_A["Public Subnet A<br/>10.0.1.0/24<br/>AZ-a"]
            PUB_B["Public Subnet B<br/>10.0.2.0/24<br/>AZ-b"]
        end
        
        subgraph "Private Subnets - App"
            PRIV_APP_A["Private App A<br/>10.0.10.0/24<br/>AZ-a"]
            PRIV_APP_B["Private App B<br/>10.0.11.0/24<br/>AZ-b"]
        end
        
        subgraph "Private Subnets - DB"
            PRIV_DB_A["Private DB A<br/>10.0.20.0/24<br/>AZ-a"]
            PRIV_DB_B["Private DB B<br/>10.0.21.0/24<br/>AZ-b"]
        end
        
        IGW[Internet Gateway]
        NAT_A[NAT Gateway A]
        NAT_B[NAT Gateway B]
        ALB_INT[Application Load Balancer]
        
        IGW --> PUB_A
        IGW --> PUB_B
        PUB_A --> NAT_A
        PUB_B --> NAT_B
        NAT_A --> PRIV_APP_A
        NAT_B --> PRIV_APP_B
        ALB_INT --> PRIV_APP_A
        ALB_INT --> PRIV_APP_B
        PRIV_APP_A --> PRIV_DB_A
        PRIV_APP_B --> PRIV_DB_B
    end
```

### 3.2 ECS Service Architecture

```mermaid
graph TB
    subgraph "ECS Cluster"
        subgraph "Service: cms-api"
            TASK1["Task 1<br/>API Container"]
            TASK2["Task 2<br/>API Container"]
            TASK3["Task N<br/>API Container"]
        end
        
        subgraph "Auto Scaling"
            ASG["Target Tracking<br/>CPU: 70%<br/>Memory: 80%"]
        end
    end
    
    ALB[Application Load Balancer] --> TASK1
    ALB --> TASK2
    ALB --> TASK3
    
    ASG -.->|Scale| TASK3
    
    subgraph "ECR"
        IMG["cms-api:latest<br/>Docker Image"]
    end
    
    IMG -.->|Pull| TASK1
    IMG -.->|Pull| TASK2
    IMG -.->|Pull| TASK3
```

### 3.3 RDS Multi-AZ Architecture

```mermaid
graph TB
    subgraph "AWS RDS - MariaDB"
        subgraph "AZ-a"
            PRIMARY["Primary Instance<br/>db.r6g.large<br/>Read/Write"]
        end
        
        subgraph "AZ-b"
            STANDBY["Standby Instance<br/>db.r6g.large<br/>Sync Replication"]
        end
        
        subgraph "Read Replicas (Optional)"
            READ1["Read Replica 1"]
            READ2["Read Replica 2"]
        end
    end
    
    APP[ECS Services] -->|Write| PRIMARY
    APP -->|Read| READ1
    APP -->|Read| READ2
    
    PRIMARY -->|Sync| STANDBY
    PRIMARY -->|Async| READ1
    PRIMARY -->|Async| READ2
    
    PRIMARY -.->|Failover| STANDBY
```

---

## 4. API Service Design

### 4.1 Service Layer Architecture

```mermaid
graph TB
    subgraph "API Layer"
        CTRL[Controllers]
    end
    
    subgraph "Service Layer"
        AUTH_SVC[AuthService]
        DEV_SVC[DeviceService]
        CNT_SVC[ContentService]
        SCH_SVC[ScheduleService]
        USR_SVC[UserService]
        ALT_SVC[AlertService]
        RPT_SVC[ReportService]
    end
    
    subgraph "Repository Layer"
        USER_REPO[UserRepository]
        DEV_REPO[DeviceRepository]
        CNT_REPO[ContentRepository]
        SCH_REPO[ScheduleRepository]
        GRP_REPO[GroupRepository]
    end
    
    subgraph "External Services"
        S3_CLIENT[S3Client]
        SQS_CLIENT[SQSClient]
        SNS_CLIENT[SNSClient]
    end
    
    CTRL --> AUTH_SVC
    CTRL --> DEV_SVC
    CTRL --> CNT_SVC
    CTRL --> SCH_SVC
    CTRL --> USR_SVC
    CTRL --> ALT_SVC
    CTRL --> RPT_SVC
    
    AUTH_SVC --> USER_REPO
    DEV_SVC --> DEV_REPO
    DEV_SVC --> GRP_REPO
    CNT_SVC --> CNT_REPO
    CNT_SVC --> S3_CLIENT
    SCH_SVC --> SCH_REPO
    USR_SVC --> USER_REPO
    ALT_SVC --> SNS_CLIENT
    RPT_SVC --> DEV_REPO
```

### 4.2 Authentication Flow

```mermaid
sequenceDiagram
    participant Client
    participant ALB
    participant AuthService
    participant Redis
    participant MariaDB
    
    Client->>ALB: POST /api/v1/auth/login
    ALB->>AuthService: Forward request
    AuthService->>MariaDB: Validate credentials
    MariaDB-->>AuthService: User data
    AuthService->>AuthService: Generate JWT
    AuthService->>Redis: Store refresh token
    AuthService-->>Client: {accessToken, refreshToken}
    
    Note over Client,AuthService: Subsequent API calls
    
    Client->>ALB: GET /api/v1/devices<br/>Authorization: Bearer {jwt}
    ALB->>AuthService: Validate JWT
    AuthService->>AuthService: Verify signature
    AuthService-->>ALB: Valid
    ALB->>DeviceService: Forward request
```

### 4.3 Device Sync Flow

```mermaid
sequenceDiagram
    participant Android as Android App
    participant ALB as Load Balancer
    participant API as API Service
    participant DB as MariaDB
    participant S3
    participant SQS
    
    Android->>ALB: POST /api/v1/sync/heartbeat<br/>{playerId, status}
    ALB->>API: Forward
    API->>DB: Update device status
    API-->>Android: {scheduleVersion, hasUpdate}
    
    alt Schedule Update Available
        Android->>ALB: GET /api/v1/sync/schedule
        ALB->>API: Forward
        API->>DB: Get schedule
        API-->>Android: {schedule, contentList}
        
        loop For each content
            Android->>S3: GET content file
            S3-->>Android: File data
        end
    end
    
    Android->>ALB: POST /api/v1/sync/log<br/>{usageData}
    ALB->>API: Forward
    API->>SQS: Queue log processing
    API-->>Android: {success}
```

### 4.4 Security Alert Flow

```mermaid
sequenceDiagram
    participant Admin as CMS Web
    participant API as API Service
    participant DB as MariaDB
    participant SNS
    participant SQS
    participant Devices as Android Apps
    
    Admin->>API: POST /api/v1/alerts/deploy<br/>{alertId, groupIds}
    API->>DB: Get target devices
    API->>SNS: Publish alert notification
    SNS->>SQS: Fan-out to queues
    API-->>Admin: {deploymentId, status}
    
    Note over Devices: Devices polling or WebSocket
    
    loop Each device heartbeat
        Devices->>API: POST /api/v1/sync/heartbeat
        API->>DB: Check active alert
        API-->>Devices: {alert: true, alertContent}
        Devices->>Devices: Display alert
    end
    
    Admin->>API: POST /api/v1/alerts/stop
    API->>DB: Clear active alert
    API->>SNS: Publish stop notification
```

---

## 5. Database Design

### 5.1 Entity Relationship Diagram

```mermaid
erDiagram
    USERS ||--o{ USER_GROUPS : belongs_to
    GROUPS ||--o{ USER_GROUPS : has
    GROUPS ||--o{ DEVICES : contains
    GROUPS ||--o{ CONTENT : owns
    GROUPS ||--o{ SCHEDULES : owns
    DEVICES ||--o{ USAGE_LOGS : generates
    CONTENT ||--o{ SCHEDULE_CONTENT : used_in
    SCHEDULES ||--o{ SCHEDULE_CONTENT : contains
    SCHEDULES ||--o{ DEPLOYMENTS : deployed_to
    DEVICES ||--o{ DEPLOYMENTS : receives
    
    USERS {
        bigint id PK
        varchar username UK
        varchar password_hash
        enum role
        varchar email
        varchar phone
        timestamp created_at
        timestamp updated_at
    }
    
    GROUPS {
        bigint id PK
        varchar name
        timestamp created_at
        timestamp updated_at
    }
    
    USER_GROUPS {
        bigint user_id FK
        bigint group_id FK
    }
    
    DEVICES {
        bigint id PK
        varchar player_id UK
        varchar name
        bigint group_id FK
        enum status
        varchar ip_address
        timestamp last_seen
        timestamp created_at
    }
    
    CONTENT {
        bigint id PK
        varchar name
        enum type
        varchar s3_key
        bigint file_size
        bigint group_id FK
        varchar thumbnail_key
        timestamp created_at
    }
    
    SCHEDULES {
        bigint id PK
        varchar name
        bigint group_id FK
        json layout
        json timing
        int version
        timestamp created_at
        timestamp updated_at
    }
    
    SCHEDULE_CONTENT {
        bigint schedule_id FK
        bigint content_id FK
        varchar zone_id
        int sequence
    }
    
    DEPLOYMENTS {
        bigint id PK
        bigint schedule_id FK
        bigint device_id FK
        enum status
        timestamp deployed_at
    }
    
    USAGE_LOGS {
        bigint id PK
        bigint device_id FK
        bigint content_id FK
        int play_seconds
        date log_date
        timestamp created_at
    }
    
    ALERTS {
        bigint id PK
        varchar name
        bigint content_id FK
        boolean is_active
        timestamp deployed_at
        timestamp stopped_at
    }
```

### 5.2 Database Schema Details

```sql
-- Users Table
CREATE TABLE users (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) NOT NULL UNIQUE,
    password_hash VARCHAR(255) NOT NULL,
    role ENUM('admin', 'manager', 'user') NOT NULL DEFAULT 'user',
    email VARCHAR(100),
    phone VARCHAR(20),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    INDEX idx_username (username),
    INDEX idx_role (role)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- Groups Table
CREATE TABLE groups (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- Devices Table
CREATE TABLE devices (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    player_id VARCHAR(100) NOT NULL UNIQUE,
    name VARCHAR(100),
    group_id BIGINT,
    status ENUM('online', 'offline') DEFAULT 'offline',
    ip_address VARCHAR(45),
    last_seen TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (group_id) REFERENCES groups(id) ON DELETE SET NULL,
    INDEX idx_player_id (player_id),
    INDEX idx_group_id (group_id),
    INDEX idx_status (status)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- Content Table
CREATE TABLE content (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    type ENUM('video', 'image', 'html', 'stream') NOT NULL,
    s3_key VARCHAR(500) NOT NULL,
    file_size BIGINT,
    group_id BIGINT,
    thumbnail_key VARCHAR(500),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (group_id) REFERENCES groups(id) ON DELETE SET NULL,
    INDEX idx_group_id (group_id),
    INDEX idx_type (type)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- Schedules Table
CREATE TABLE schedules (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    group_id BIGINT,
    layout JSON NOT NULL,
    timing JSON,
    version INT DEFAULT 1,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (group_id) REFERENCES groups(id) ON DELETE SET NULL,
    INDEX idx_group_id (group_id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

---

## 6. API Specifications

### 6.1 API Request/Response Format

```mermaid
graph LR
    subgraph "Request"
        REQ_HEADER["Headers:<br/>Authorization: Bearer {jwt}<br/>Content-Type: application/json"]
        REQ_BODY["Body: JSON"]
    end
    
    subgraph "Response"
        RES_SUCCESS["Success (2xx):<br/>{data, meta}"]
        RES_ERROR["Error (4xx/5xx):<br/>{error, message, details}"]
    end
    
    REQ_HEADER --> API[API Endpoint]
    REQ_BODY --> API
    API --> RES_SUCCESS
    API --> RES_ERROR
```

### 6.2 API Response Structures

**Success Response:**
```json
{
  "success": true,
  "data": { },
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 100
  }
}
```

**Error Response:**
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": [
      {"field": "name", "message": "Name is required"}
    ]
  }
}
```

### 6.3 API Endpoint Summary

```mermaid
graph TB
    subgraph "Authentication"
        A1[POST /auth/login]
        A2[POST /auth/logout]
        A3[POST /auth/refresh]
    end
    
    subgraph "Devices"
        D1[GET /devices]
        D2[POST /devices]
        D3[GET /devices/:id]
        D4[PUT /devices/:id]
        D5[DELETE /devices/:id]
    end
    
    subgraph "Content"
        C1[GET /content]
        C2[POST /content]
        C3[DELETE /content/:id]
    end
    
    subgraph "Schedules"
        S1[GET /schedules]
        S2[POST /schedules]
        S3[PUT /schedules/:id]
        S4[POST /schedules/:id/deploy]
    end
    
    subgraph "Device Sync"
        Y1[POST /sync/register]
        Y2[POST /sync/heartbeat]
        Y3[GET /sync/schedule]
    end
```

---

## 7. Security Design

### 7.1 Security Architecture

```mermaid
graph TB
    subgraph "Internet"
        CLIENT[Clients]
    end
    
    subgraph "Edge Security"
        WAF[AWS WAF<br/>SQL Injection<br/>XSS Protection]
        SHIELD[AWS Shield<br/>DDoS Protection]
    end
    
    subgraph "Transport Security"
        TLS[TLS 1.3<br/>Certificate Manager]
    end
    
    subgraph "Application Security"
        JWT[JWT Authentication<br/>RS256 Signing]
        RBAC[Role-Based Access<br/>Control]
        RATE[Rate Limiting]
    end
    
    subgraph "Data Security"
        ENC_TRANSIT[Encryption in Transit<br/>TLS]
        ENC_REST[Encryption at Rest<br/>AES-256]
        SM[Secrets Manager<br/>Credentials]
    end
    
    CLIENT --> WAF
    WAF --> SHIELD
    SHIELD --> TLS
    TLS --> JWT
    JWT --> RBAC
    RBAC --> RATE
    RATE --> ENC_TRANSIT
    ENC_TRANSIT --> ENC_REST
```

### 7.2 JWT Token Structure

```mermaid
graph LR
    subgraph "JWT Token"
        HEADER["Header<br/>{alg: RS256, typ: JWT}"]
        PAYLOAD["Payload<br/>{sub, role, groups, exp, iat}"]
        SIG["Signature<br/>RS256(header.payload, privateKey)"]
    end
    
    HEADER --> PAYLOAD --> SIG
```

---

## 8. Scalability Design

### 8.1 Auto-Scaling Configuration

```mermaid
graph TB
    subgraph "CloudWatch Metrics"
        CPU[CPU Utilization]
        MEM[Memory Utilization]
        REQ[Request Count]
    end
    
    subgraph "Scaling Policies"
        SCALE_OUT[Scale Out<br/>CPU > 70%<br/>Add 2 tasks]
        SCALE_IN[Scale In<br/>CPU < 30%<br/>Remove 1 task]
    end
    
    subgraph "ECS Service"
        MIN[Min: 2 tasks]
        DESIRED[Desired: 4 tasks]
        MAX[Max: 20 tasks]
    end
    
    CPU --> SCALE_OUT
    CPU --> SCALE_IN
    MEM --> SCALE_OUT
    SCALE_OUT --> DESIRED
    SCALE_IN --> DESIRED
    MIN --> DESIRED
    DESIRED --> MAX
```

### 8.2 Content Delivery Architecture

```mermaid
graph TB
    subgraph "Content Upload Flow"
        WEB[CMS Web] -->|Upload| API[API Service]
        API -->|Store| S3[S3 Bucket]
        API -->|Save metadata| DB[(MariaDB)]
    end
    
    subgraph "Content Delivery Flow"
        ANDROID[Android App] -->|Get URL| API2[API Service]
        API2 -->|Signed URL| ANDROID
        ANDROID -->|Download| CF[CloudFront]
        CF -->|Cache miss| S3
    end
```

---

## 9. Monitoring & Logging

### 9.1 Observability Stack

```mermaid
graph TB
    subgraph "Application"
        APP[ECS Services]
    end
    
    subgraph "Logging"
        CW_LOGS[CloudWatch Logs]
    end
    
    subgraph "Metrics"
        CW_METRICS[CloudWatch Metrics]
    end
    
    subgraph "Tracing"
        XRAY[AWS X-Ray]
    end
    
    subgraph "Alerting"
        ALARM[CloudWatch Alarms]
        SNS_ALERT[SNS Notifications]
    end
    
    APP -->|Logs| CW_LOGS
    APP -->|Metrics| CW_METRICS
    APP -->|Traces| XRAY
    CW_METRICS --> ALARM
    ALARM --> SNS_ALERT
```

### 9.2 Key Metrics

| Metric | Threshold | Action |
|--------|-----------|--------|
| API Response Time (p95) | > 500ms | Alert |
| Error Rate | > 1% | Alert |
| CPU Utilization | > 70% | Scale out |
| Database Connections | > 80% | Alert |
| S3 Request Errors | > 0.1% | Alert |

---

## 10. Deployment Design

### 10.1 CI/CD Pipeline

```mermaid
graph LR
    subgraph "Source"
        GIT[GitHub/GitLab]
    end
    
    subgraph "Build"
        CB[CodeBuild<br/>Docker Build]
    end
    
    subgraph "Registry"
        ECR[ECR<br/>Container Registry]
    end
    
    subgraph "Deploy"
        CD[CodeDeploy<br/>Blue/Green]
    end
    
    subgraph "Runtime"
        ECS[ECS Fargate]
    end
    
    GIT -->|Trigger| CB
    CB -->|Push| ECR
    ECR -->|Pull| CD
    CD -->|Deploy| ECS
```

### 10.2 Blue/Green Deployment

```mermaid
graph TB
    subgraph "Production"
        ALB[Load Balancer]
        
        subgraph "Blue (Current)"
            BLUE1[Task v1.0]
            BLUE2[Task v1.0]
        end
        
        subgraph "Green (New)"
            GREEN1[Task v1.1]
            GREEN2[Task v1.1]
        end
    end
    
    ALB -->|100%| BLUE1
    ALB -->|100%| BLUE2
    ALB -.->|0%| GREEN1
    ALB -.->|0%| GREEN2
    
    NOTE[Traffic shift:<br/>100% Blue → 100% Green]
```

---

## 11. Disaster Recovery

### 11.1 Backup Strategy

```mermaid
graph TB
    subgraph "RDS Backup"
        AUTO[Automated Backups<br/>Daily, 7-day retention]
        SNAP[Manual Snapshots<br/>Before major changes]
        PITR[Point-in-Time Recovery<br/>5-minute granularity]
    end
    
    subgraph "S3 Backup"
        VERSION[Versioning<br/>All objects]
        REPLICA[Cross-Region Replication<br/>DR region]
    end
    
    subgraph "Configuration"
        IaC[Infrastructure as Code<br/>Terraform/CloudFormation]
    end
```

### 11.2 Recovery Objectives

| Metric | Target |
|--------|--------|
| RTO (Recovery Time Objective) | < 1 hour |
| RPO (Recovery Point Objective) | < 5 minutes |
| Failover Time (RDS) | < 2 minutes |

---

## 12. Appendix

### 12.1 Technology Stack

| Layer | Technology |
|-------|------------|
| Runtime | Node.js 20 LTS / Java 17 |
| Framework | Express.js / Spring Boot |
| Database | MariaDB 10.11 |
| Cache | Redis 7 |
| Container | Docker |
| Orchestration | AWS ECS Fargate |
| CDN | AWS CloudFront |
| Storage | AWS S3 |
| Secrets | AWS Secrets Manager |
| Monitoring | AWS CloudWatch |

### 12.2 AWS Resource Naming Convention

| Resource | Pattern | Example |
|----------|---------|---------|
| ECS Cluster | `{env}-cms-cluster` | `prod-cms-cluster` |
| ECS Service | `{env}-cms-api` | `prod-cms-api` |
| RDS Instance | `{env}-cms-db` | `prod-cms-db` |
| S3 Bucket | `{env}-cms-media-{account}` | `prod-cms-media-123456` |
| ALB | `{env}-cms-alb` | `prod-cms-alb` |

---

## Document Approval

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Technical Lead | | | |
| DevOps Lead | | | |
| Security Lead | | | |
| AWS Architect | | | |
