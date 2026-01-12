# Business Requirements Document (BRD)
## Podoh Web Application

**Document Version:** 2.0  
**Date:** January 9, 2026  
**Product:** Podoh Platform

---

## 1. Executive Summary

### 1.1 Purpose
This document defines the business requirements for the Podoh Web Application - a browser-based single-page application (SPA) that provides the user interface for content management, schedule design, and device management for PVM devices.

### 1.2 Product Vision
Deliver an intuitive web-based content management platform with a familiar user interface (maintaining consistency with the existing system) that empowers non-technical users to design professional Podoh layouts, manage device networks, and deploy content with minimal training.

### 1.3 Architecture Position
The Podoh Web Application is the frontend component of a four-tier architecture:
- **Podoh Web** - This component (SPA deployed on AWS)
- **Podoh API** - Backend RESTful services (Docker containers on AWS ECS)
- **Database** - MariaDB on AWS RDS
- **Podoh Player** - Android application on PVM monitor boards

### 1.4 Target Users
- Marketing managers creating promotional content
- Retail operations managing store displays
- Security administrators deploying emergency alerts
- IT administrators managing device infrastructure
- Content designers creating multi-zone layouts

---

## 2. Business Objectives

| ID | Objective | Success Metric |
|----|-----------|----------------|
| BO-001 | Maintain familiar UI | Existing users productive immediately |
| BO-002 | Enable self-service content management | Users create and deploy content without IT support |
| BO-003 | Support multi-zone layout design | Create complex layouts with drag-and-drop interface |
| BO-004 | Provide flexible scheduling | Schedule content by time, day, and duration |
| BO-005 | Enable usage tracking | Generate billing reports based on content play time |
| BO-006 | Support emergency broadcasting | Deploy security alerts in < 10 seconds |
| BO-007 | Multi-language support | Interface available in 5+ languages |

---

## 3. Functional Requirements

### 3.1 User Authentication & Management

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-AUTH-001 | Login Authentication | Must Have | Username/password login via API |
| FR-AUTH-002 | JWT Token Management | Must Have | Store and refresh JWT tokens |
| FR-AUTH-003 | Role-Based UI | Must Have | Show/hide features based on user role |
| FR-AUTH-004 | Session Timeout | Must Have | Auto-logout after inactivity |
| FR-AUTH-005 | Password Change | Must Have | Users can change own password |
| FR-AUTH-006 | Remember Me | Should Have | Optional persistent login |

#### 3.1.1 Role Permissions Matrix

| Permission | Administrator | Manager | User |
|------------|--------------|---------|------|
| Create/Delete Users | ✓ | ✗ | ✗ |
| Register Devices | ✓ | ✓ | ✓ |
| Add Contents | ✓ | ✓ | ✓ |
| Create/Edit Schedules | ✓ | ✓ | ✗ |
| Deploy Schedules | ✓ | ✓ | ✓ |
| Server Settings | ✓ | ✗ | ✗ |

### 3.2 Device Management (Player)

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-PLY-001 | Manual Device Registration | Must Have | Register device by Player ID |
| FR-PLY-002 | Device Naming | Must Have | Assign custom names to devices |
| FR-PLY-003 | Group Assignment | Must Have | Assign devices to groups |
| FR-PLY-004 | Device Status Display | Must Have | Show online/offline status icons |
| FR-PLY-005 | Device Editing | Must Have | Modify device properties |
| FR-PLY-006 | Device List View | Must Have | Display all registered devices |
| FR-PLY-007 | Usage Log Access | Must Have | View content playback history |
| FR-PLY-008 | Real-time Status | Should Have | WebSocket-based status updates |

### 3.3 Group Management

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-GRP-001 | Create Groups | Must Have | Add new device groups |
| FR-GRP-002 | Group Naming | Must Have | Assign descriptive group names |
| FR-GRP-003 | Group Listing | Must Have | Display all groups |
| FR-GRP-004 | Group Deployment | Must Have | Deploy to entire group at once |
| FR-GRP-005 | Group Filtering | Should Have | Filter devices by group |

### 3.4 Content Management

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-CNT-001 | Content Upload | Must Have | Upload files to S3 via API |
| FR-CNT-002 | Video Support | Must Have | Accept MP4, AVI, MKV formats |
| FR-CNT-003 | Image Support | Must Have | Accept JPG, PNG, BMP formats |
| FR-CNT-004 | HTML Content | Must Have | Add web URLs as content |
| FR-CNT-005 | Streaming Media | Must Have | Add RTSP/HTTP stream URLs |
| FR-CNT-006 | Content Preview | Must Have | Double-click to preview content |
| FR-CNT-007 | Content Deletion | Must Have | Remove content from database |
| FR-CNT-008 | Group Assignment | Must Have | Assign content to groups |
| FR-CNT-009 | Icon View | Must Have | Display content as thumbnails |
| FR-CNT-010 | List View | Must Have | Display content in list format |
| FR-CNT-011 | Search | Should Have | Search content by name |
| FR-CNT-012 | Upload Progress | Must Have | Show upload progress indicator |

### 3.5 Schedule Creation (Ad Design)

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-SCH-001 | New Schedule | Must Have | Create new schedule/ad |
| FR-SCH-002 | Schedule Naming | Must Have | Assign name to schedule |
| FR-SCH-003 | Group Assignment | Must Have | Assign schedule to group |
| FR-SCH-004 | Schedule Preview | Must Have | Preview schedule in mock PVM |
| FR-SCH-005 | Schedule Save | Must Have | Save schedule configuration |
| FR-SCH-006 | Schedule Edit | Must Have | Modify existing schedules |
| FR-SCH-007 | Schedule List | Must Have | Display all schedules |
| FR-SCH-008 | Schedule Deploy | Must Have | Push schedule to devices |

### 3.6 Layout Design

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-LAY-001 | Video/Image Zone | Must Have | Add video/image content zone |
| FR-LAY-002 | Image Zones (1-3) | Must Have | Up to 3 separate image zones |
| FR-LAY-003 | Logo Zone | Must Have | Add logo overlay |
| FR-LAY-004 | Clock Widget | Must Have | Add analog/digital clock |
| FR-LAY-005 | Subtitle Zone | Must Have | Add text subtitle |
| FR-LAY-006 | News Ticker (1-5) | Must Have | Up to 5 scrolling ticker zones |
| FR-LAY-007 | Camera Feed | Must Have | Add live camera input zone |
| FR-LAY-008 | Wallpaper | Must Have | Set background image |
| FR-LAY-009 | HTML Zone | Must Have | Add web content zone |
| FR-LAY-010 | Drag-and-Drop | Must Have | Position zones by dragging |
| FR-LAY-011 | Resize Zones | Must Have | Resize from corners |
| FR-LAY-012 | Numeric Positioning | Must Have | Set Left/Top/Right/Bottom % |
| FR-LAY-013 | Alignment Tools | Must Have | Top/Bottom/Left/Right alignment |

#### 3.6.1 Content Layer Hierarchy (Bottom to Top)
1. Wallpaper
2. HTML
3. Image 1
4. Image 2
5. Image 3
6. Logo
7. Clock
8. Subtitle
9. News Ticker 1-5
10. Video/Image
11. Camera Input

### 3.7 Aspect Ratio & Orientation

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-ASP-001 | 16:9 Ratio | Must Have | Standard widescreen ratio |
| FR-ASP-002 | 4:3 Ratio | Must Have | Traditional ratio |
| FR-ASP-003 | 5:4 Ratio | Should Have | Near-square ratio |
| FR-ASP-004 | 16:4.5 Ratio | Should Have | Ultra-wide ratio |
| FR-ORI-001 | 0° Rotation | Must Have | Default portrait orientation |
| FR-ORI-002 | 90° Rotation | Must Have | Landscape right |
| FR-ORI-003 | 180° Rotation | Must Have | Inverted portrait |
| FR-ORI-004 | 270° Rotation | Must Have | Landscape left |

### 3.8 Schedule Timing

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-TIM-001 | Playback Start Time | Must Have | Set daily start time (24hr format) |
| FR-TIM-002 | Playback End Time | Must Have | Set daily end time (24hr format) |
| FR-TIM-003 | Day Selection | Must Have | Select specific days of week |
| FR-TIM-004 | Repeat Option | Must Have | Enable weekly repeat |
| FR-TIM-005 | Sub-Schedules | Must Have | Multiple time slots per day |
| FR-TIM-006 | Continuous Play | Must Have | Option for 24/7 playback |
| FR-TIM-007 | Image Duration | Must Have | Set display time for images (default 10s) |

### 3.9 Content Sequencing

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-SEQ-001 | Video Playlist | Must Have | Multiple videos play in sequence |
| FR-SEQ-002 | Image Slideshow | Must Have | Multiple images rotate with timing |
| FR-SEQ-003 | Loop Playback | Must Have | Content loops continuously |

### 3.10 Deployment

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-DEP-001 | Deploy Button | Must Have | One-click deployment |
| FR-DEP-002 | Device Selection | Must Have | Select target devices |
| FR-DEP-003 | Group Selection | Must Have | Deploy to entire group |
| FR-DEP-004 | Progress Display | Must Have | Show deployment progress |
| FR-DEP-005 | Confirmation | Must Have | Confirm successful deployment |

### 3.11 Usage Reporting

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-RPT-001 | Device Selection | Must Have | Select device for report |
| FR-RPT-002 | Content List | Must Have | Show all content played |
| FR-RPT-003 | Play Duration | Must Have | Display time in seconds and HH:MM:SS |
| FR-RPT-004 | Media Type | Must Have | Identify content type |
| FR-RPT-005 | Excel Export | Must Have | Export to XLS format |

### 3.12 Security Alert System

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-ALT-001 | Alert Content Upload | Must Have | Upload alert images/videos |
| FR-ALT-002 | Alert Selection | Must Have | Select alert content |
| FR-ALT-003 | Group Targeting | Must Have | Target specific groups or all |
| FR-ALT-004 | Deploy Alert | Must Have | Instant alert deployment |
| FR-ALT-005 | Stop Alert | Must Have | Cancel alert, resume normal |
| FR-ALT-006 | Audio Support | Should Have | Alert videos can include audio |

### 3.13 Server Configuration

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-CFG-001 | Language Selection | Must Have | Change interface language |
| FR-CFG-002 | API Endpoint Config | Must Have | Configure API base URL |
| FR-CFG-003 | Auto Sync Option | Must Have | Enable automatic media sync |
| FR-CFG-004 | Player Settings | Must Have | Configure player defaults |

### 3.14 Internationalization

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-I18N-001 | English | Must Have | English interface |
| FR-I18N-002 | Spanish | Must Have | Spanish interface |
| FR-I18N-003 | French | Must Have | French interface |
| FR-I18N-004 | Russian | Should Have | Russian interface |
| FR-I18N-005 | Portuguese | Should Have | Portuguese interface |
| FR-I18N-006 | Korean | Should Have | Korean interface |

---

## 4. Non-Functional Requirements

### 4.1 Usability

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-USE-001 | Learning Curve | Existing users productive immediately |
| NFR-USE-002 | Task Completion | Create and deploy ad in < 10 minutes |
| NFR-USE-003 | Visual Feedback | Real-time preview of layout changes |
| NFR-USE-004 | Error Messages | Clear, actionable error messages |
| NFR-USE-005 | UI Consistency | Match existing system UI/UX |

### 4.2 Performance

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-PERF-001 | Initial Load | < 3 seconds (cached resources) |
| NFR-PERF-002 | API Response | < 500ms perceived latency |
| NFR-PERF-003 | File Upload | Progress indicator, resumable |
| NFR-PERF-004 | Preview Rendering | < 1 second for layout preview |

### 4.3 Compatibility

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-COMP-001 | Primary Browser | Chrome (fully supported) |
| NFR-COMP-002 | Secondary Browsers | Firefox, Edge (supported) |
| NFR-COMP-003 | Screen Resolution | 1920x1080 minimum recommended |
| NFR-COMP-004 | Mobile | Responsive design (view only) |

### 4.4 Deployment

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-DEP-001 | Hosting | AWS S3 + CloudFront |
| NFR-DEP-002 | CDN | Global edge distribution |
| NFR-DEP-003 | SSL | HTTPS only |
| NFR-DEP-004 | Versioning | Cache-busting for updates |

---

## 5. User Interface Requirements

### 5.1 Main Navigation (Unchanged from Current System)

| Tab | Description |
|-----|-------------|
| Player | Device management and registration |
| Contents | Content library management |
| Schedule | Schedule creation and deployment |
| Security Alert | Emergency alert management |
| Management | User and server configuration |

### 5.2 Key UI Components (Matching Current System)

| Component | Description |
|-----------|-------------|
| Mock PVM Display | Visual preview of layout design |
| Content Grid | Thumbnail view of content library |
| Device List | Table of registered devices |
| Schedule Timeline | Visual representation of time slots |
| Deployment Dialog | Device/group selection for deployment |

---

## 6. User Stories

### 6.1 Marketing Manager
> "As a marketing manager, I want to use the same familiar interface so that I don't need to learn a new system."

### 6.2 Store Manager
> "As a store manager, I want to schedule different content for morning and evening hours so that I can target different customer demographics."

### 6.3 Content Designer
> "As a content designer, I want to position multiple content zones precisely so that I can create professional-looking layouts."

### 6.4 Operations Director
> "As an operations director, I want to deploy the same content to all stores in a region so that branding is consistent across locations."

### 6.5 Security Manager
> "As a security manager, I want to instantly display evacuation instructions on all displays so that building occupants receive emergency guidance."

### 6.6 Finance Manager
> "As a finance manager, I want to export content play time reports so that I can bill advertisers accurately."

### 6.7 IT Administrator
> "As an IT administrator, I want the web application to be fast and reliable regardless of how many users are accessing it."

---

## 7. Technical Requirements

### 7.1 Frontend Technology Stack

| Component | Technology |
|-----------|------------|
| Framework | React 18+ or Vue 3+ |
| State Management | Redux/Zustand or Pinia |
| HTTP Client | Axios with interceptors |
| Styling | CSS-in-JS or Tailwind CSS |
| Build Tool | Vite or Webpack 5 |
| Testing | Jest + React Testing Library |

### 7.2 API Integration

| Requirement | Description |
|-------------|-------------|
| Base URL | Configurable API endpoint |
| Authentication | JWT bearer tokens |
| Error Handling | Global error interceptor |
| Retry Logic | Automatic retry on 5xx errors |
| Caching | Local storage for user preferences |

---

## 8. Constraints

| ID | Constraint | Description |
|----|------------|-------------|
| CON-001 | UI Consistency | Must match existing system appearance |
| CON-002 | Browser Support | Optimized for Chrome |
| CON-003 | Network Dependency | Requires internet connection |
| CON-004 | File Size | Maximum 500MB per upload |
| CON-005 | Concurrent Editing | No real-time collaboration |

---

## 9. Assumptions

1. Users are familiar with existing CMS interface
2. Users have Chrome browser installed
3. Network connectivity is reliable
4. Content files are in supported formats
5. Backend API services are available

---

## 10. Dependencies

| ID | Dependency | Description |
|----|------------|-------------|
| DEP-001 | Podoh API | Backend API must be available |
| DEP-002 | AWS S3 | Content storage |
| DEP-003 | AWS CloudFront | Content delivery |
| DEP-004 | Chrome Browser | Primary browser support |
| DEP-005 | Podoh Player | Registered devices for deployment |

---

## 11. Acceptance Criteria

| ID | Criteria | Verification Method |
|----|----------|---------------------|
| AC-001 | UI matches current system | Visual comparison |
| AC-002 | Login with JWT works | Authentication test |
| AC-003 | Content uploads to S3 | Upload test |
| AC-004 | Layout preview accurate | Visual comparison |
| AC-005 | Schedule deploys correctly | End-to-end test |
| AC-006 | Excel export works | Export test |
| AC-007 | Alert deploys in < 10s | Timing test |
| AC-008 | Multi-language works | Localization test |

---

## 12. Glossary

| Term | Definition |
|------|------------|
| SPA | Single Page Application |
| JWT | JSON Web Token |
| CDN | Content Delivery Network |
| S3 | AWS Simple Storage Service |
| CloudFront | AWS CDN service |
| Mock PVM | Visual preview of device display |

---

## Document Approval

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Product Owner | | | |
| UX Lead | | | |
| Technical Lead | | | |
| QA Lead | | | |
