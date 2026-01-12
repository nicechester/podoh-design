# Business Requirements Document (BRD)
## Podoh PVM - Public View Monitor

**Document Version:** 1.0  
**Date:** January 9, 2026  
**Product:** Podoh PVM Series (PVM-P4B3/P4W3, PVM-P5B3/P5W3)

---

## 1. Executive Summary

### 1.1 Purpose
This document defines the business requirements for the Podoh Public View Monitor (PVM) - an Android-based display system with integrated IP camera for retail, commercial, and security applications.

### 1.2 Product Vision
Deliver an all-in-one Podoh solution that combines high-quality display, content management capabilities, and integrated surveillance in a single device optimized for portrait orientation deployment.

### 1.3 Target Market
- Retail stores and shopping centers
- Banks and financial institutions
- Corporate lobbies and reception areas
- Transportation hubs
- Healthcare facilities
- Educational institutions

---

## 2. Business Objectives

| ID | Objective | Success Metric |
|----|-----------|----------------|
| BO-001 | Provide integrated Podoh display with surveillance | Single device replaces separate display + camera installations |
| BO-002 | Enable remote content management | 100% of content updates deployable remotely via Podoh |
| BO-003 | Support multiple installation orientations | Portrait and landscape mounting with camera rotation |
| BO-004 | Ensure enterprise-grade reliability | 3-year warranty, commercial-grade components |
| BO-005 | Enable security alert broadcasting | Real-time emergency notification capability |

---

## 3. Functional Requirements

### 3.1 Display System

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-DSP-001 | LED Backlit Display | Must Have | 32" or 43" LED backlit monitor with 1080x1920 resolution |
| FR-DSP-002 | Portrait Orientation | Must Have | Native 9:16 aspect ratio support |
| FR-DSP-003 | Brightness Control | Must Have | 250 cd/m² brightness with adjustable settings (0-100) |
| FR-DSP-004 | Contrast Adjustment | Must Have | 4000:1 (32") / 1200:1 (43") contrast ratio |
| FR-DSP-005 | Wide Viewing Angle | Must Have | 89°/89° horizontal and vertical viewing angles |
| FR-DSP-006 | Color Temperature | Must Have | Warm/Normal/Cool presets + custom RGB adjustment |
| FR-DSP-007 | Picture Modes | Must Have | Mild/User/Dynamic/Standard presets |
| FR-DSP-008 | Aspect Ratio Options | Should Have | 16:9 and 4:3 display modes |
| FR-DSP-009 | Noise Reduction | Should Have | Image noise reduction processing |
| FR-DSP-010 | Burn-in Prevention | Must Have | Periodic image retention prevention |

### 3.2 Android Operating System

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-AND-001 | Android Platform | Must Have | Android-based operating system for content playback |
| FR-AND-002 | Podoh Player App | Must Have | Pre-installed Podoh Player application for content management |
| FR-AND-003 | Settings Access | Must Have | Full Android settings accessible via mouse/remote |
| FR-AND-004 | App Installation | Should Have | Ability to install additional Android applications |
| FR-AND-005 | Factory Reset | Must Have | Reset capability to restore default settings |

### 3.3 Integrated Camera System

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-CAM-001 | 2MP IP Camera | Must Have | Sony IMX290 sensor with Hi3516D chipset |
| FR-CAM-002 | Resolution | Must Have | 1080x1920 (portrait) video capture |
| FR-CAM-003 | Low Light Performance | Must Have | 0.01 Lux color / 0.001 Lux B&W minimum illumination |
| FR-CAM-004 | Day/Night Mode | Must Have | Automatic IR cut filter switching |
| FR-CAM-005 | Varifocal Lens | Must Have | 2.8-12mm adjustable focal length |
| FR-CAM-006 | WDR Support | Must Have | Wide Dynamic Range for high contrast scenes |
| FR-CAM-007 | Video Encoding | Must Have | H.264 BP/HP/MP and MJPEG encoding |
| FR-CAM-008 | Dual Stream | Must Have | Main stream (1080p) + sub stream (720p/D1/VGA) |
| FR-CAM-009 | ONVIF Support | Must Have | ONVIF 2.4 protocol compatibility |
| FR-CAM-010 | IP Output | Must Have | RJ-45 network output for camera stream |
| FR-CAM-011 | CVBS Output | Must Have | Analog video output option |
| FR-CAM-012 | Motion Detection | Should Have | Built-in motion detection capability |
| FR-CAM-013 | Camera Rotation | Must Have | Physical 90°/180°/270° rotation support |
| FR-CAM-014 | Recording Indicator | Should Have | LED indicator with color options (red/blue/purple/off) |

### 3.4 Network Connectivity

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-NET-001 | Ethernet Port | Must Have | RJ-45 10/100 Base-T for CMS connection |
| FR-NET-002 | Wi-Fi Support | Must Have | Wireless network connectivity |
| FR-NET-003 | DHCP Support | Must Have | Automatic IP address assignment |
| FR-NET-004 | Static IP | Must Have | Manual IP configuration capability |
| FR-NET-005 | Network Protocols | Must Have | TCP/IP, UDP, HTTP, FTP, DHCP, DNS, RTSP support |
| FR-NET-006 | Concurrent Users | Should Have | Up to 10 simultaneous camera stream viewers |

### 3.5 Input/Output Interfaces

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-IO-001 | USB Ports | Must Have | 3x USB input ports for peripherals/media |
| FR-IO-002 | Micro SD Card | Must Have | TF card slot for local storage |
| FR-IO-003 | Audio Output | Must Have | 3.5mm audio output jack |
| FR-IO-004 | Built-in Speakers | Must Have | 2x 5W integrated speakers |
| FR-IO-005 | AC Power Input | Must Have | AC 110-240V, 50/60Hz universal input |
| FR-IO-006 | DC Power Input | Must Have | DC 24V 3.0A input with polarity protection |
| FR-IO-007 | DC Power Output | Should Have | DC 12V output for accessories |

### 3.6 Content Playback

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-CNT-001 | Video Formats | Must Have | MP4, AVI, MKV file format support |
| FR-CNT-002 | Image Formats | Must Have | JPG, PNG, BMP file format support |
| FR-CNT-003 | HTML Content | Must Have | Web-based content rendering |
| FR-CNT-004 | Streaming Media | Must Have | RTSP/HTTP streaming playback |
| FR-CNT-005 | Content Scheduling | Must Have | Time-based content scheduling |
| FR-CNT-006 | Multi-Zone Layout | Must Have | Multiple content zones on single screen |
| FR-CNT-007 | Camera Feed Display | Must Have | Live camera feed as content zone |

### 3.7 Remote Control & OSD

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-RC-001 | IR Remote Control | Must Have | Infrared remote for display settings |
| FR-RC-002 | CMS Remote Control | Must Have | Separate remote for Android/CMS navigation |
| FR-RC-003 | OSD Menu | Must Have | On-screen display for picture/sound/system settings |
| FR-RC-004 | Multi-Language OSD | Must Have | English, Spanish, French, Russian, Portuguese |
| FR-RC-005 | Physical Buttons | Must Have | Power, Reset, Exit, Menu, Navigation buttons |

---

## 4. Non-Functional Requirements

### 4.1 Performance

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-PERF-001 | Display Response Time | < 6.5ms (32") / < 8ms (43") |
| NFR-PERF-002 | Video Frame Rate | 30fps (NTSC) / 25fps (PAL) |
| NFR-PERF-003 | Boot Time | < 60 seconds to content display |
| NFR-PERF-004 | Content Sync Time | < 5 minutes for typical schedule update |

### 4.2 Environmental

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-ENV-001 | Operating Temperature | -10°C to 50°C (14°F to 122°F) |
| NFR-ENV-002 | Operating Humidity | 20% to 70% RH |
| NFR-ENV-003 | Power Consumption | ≤ 50W (32") / ≤ 68W (43") |

### 4.3 Physical

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-PHY-001 | Case Material | Metal enclosure |
| NFR-PHY-002 | VESA Mount | 200mm x 200mm pattern |
| NFR-PHY-003 | Weight (32") | Net: 18.0 kg / Gross: 23.0 kg |
| NFR-PHY-004 | Weight (43") | Net: 24.4 kg / Gross: 32.0 kg |

### 4.4 Compliance & Certification

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-CERT-001 | FCC Compliance | Class A digital device |
| NFR-CERT-002 | CE Certification | European conformity |
| NFR-CERT-003 | RoHS Compliance | Hazardous substance restrictions |

### 4.5 Reliability & Support

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-REL-001 | Warranty Period | 3 years |
| NFR-REL-002 | Pixel Defect Policy | Per published standards |
| NFR-REL-003 | MTBF | Commercial-grade reliability |

---

## 5. User Stories

### 5.1 Retail Manager
> "As a retail store manager, I want to display promotional content and monitor customer activity from a single device so that I can reduce equipment costs and simplify installation."

### 5.2 Security Administrator
> "As a security administrator, I want to broadcast emergency alerts to all displays instantly so that I can ensure rapid communication during security incidents."

### 5.3 Content Manager
> "As a content manager, I want to schedule different advertisements for different times of day so that I can target content to appropriate audiences."

### 5.4 IT Administrator
> "As an IT administrator, I want to remotely update content on all displays so that I don't need to physically visit each location."

### 5.5 Installer
> "As an installation technician, I want flexible mounting options (portrait/landscape) so that I can adapt to various site requirements."

---

## 6. Constraints

| ID | Constraint | Description |
|----|------------|-------------|
| CON-001 | Network Dependency | Device requires internet for initial activation and content updates |
| CON-002 | Wiring Distance | DC 24V wiring limited to 200ft with 18AWG cable |
| CON-003 | Wi-Fi Limitations | Wireless performance affected by physical barriers |
| CON-004 | Browser Compatibility | Camera web interface optimized for IE; limited Firefox/Chrome support |
| CON-005 | Hot-spot Restriction | Mobile hotspot connections not recommended |

---

## 7. Assumptions

1. Installation locations have reliable network infrastructure
2. Qualified personnel available for initial setup and configuration
3. Podoh server infrastructure deployed and accessible
4. Adequate power supply available at installation points
5. VESA-compatible mounting hardware available

---

## 8. Dependencies

| ID | Dependency | Description |
|----|------------|-------------|
| DEP-001 | Podoh Server | Requires Podoh server for content management |
| DEP-002 | Network Infrastructure | Requires LAN/WAN connectivity |
| DEP-003 | Power Infrastructure | Requires AC 110-240V or DC 24V power |
| DEP-004 | Mounting Hardware | Optional mounts (CM-308, CMKiT-02, AM02-A) |

---

## 9. Acceptance Criteria

| ID | Criteria | Verification Method |
|----|----------|---------------------|
| AC-001 | Display shows content at specified resolution | Visual inspection |
| AC-002 | Camera stream accessible via ONVIF | Protocol testing |
| AC-003 | Content updates deploy within 5 minutes | Timing test |
| AC-004 | Device operates within temperature range | Environmental testing |
| AC-005 | All OSD functions accessible | Functional testing |
| AC-006 | Security alerts display within 10 seconds | Response time testing |

---

## 10. Glossary

| Term | Definition |
|------|------------|
| PVM | Public View Monitor |
| Podoh | Content Management Platform for PVM displays |
| OSD | On-Screen Display |
| DID | Digital Information Display |
| WDR | Wide Dynamic Range |
| ONVIF | Open Network Video Interface Forum |
| VESA | Video Electronics Standards Association |

---

## Document Approval

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Product Owner | | | |
| Technical Lead | | | |
| QA Lead | | | |
| Business Stakeholder | | | |

