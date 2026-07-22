# Secure Substation 1.0 Design Guide Digest

Source document:

- `/Users/marcusmi/Library/CloudStorage/OneDrive-Cisco/00.IOT Solutions Team/Cursor/marcusmi/CVD 3.0/Secure_Substation_1_0_DG_TOC.docx`

Created for:

- CVD 3.0 work folder
- Secure Substation CVD content reuse, restructuring, and gap tracking

Extraction date:

- 2026-07-22

## Purpose

This Markdown file digests the Secure Substation 1.0 Design Guide draft into reusable content for the Secure Substation CVD 3.0 project. It is not a verbatim DOCX conversion. It captures the chapter structure, design intent, reusable technical guidance, planning numbers, and known gaps so the material can be folded into the CVD 3.0 HTML and companion Markdown files.

The source document is a mixed table-of-contents and draft-content document. Some chapters contain substantial authored content, while others are placeholders or outlines with assigned owners.

## CVD 3.0 Content Mapping

| CVD 3.0 section | Reusable source content | Notes |
| --- | --- | --- |
| Overview | Chapter 1 outline, Chapter 2 solution overview, objectives, security drivers, deployment profiles, operations and migration | Good source for explaining why the CVD matters, target audience, and utility drivers |
| Architecture | Chapter 3 reference architecture, Chapter 5 WAN edge aggregation, service VPN model, traffic profiles, hub-and-spoke topology | Strong reusable content for topology, service segmentation, SD-WAN hubs, and traffic classes |
| Design Guide | Chapter 4 security design principles, Chapter 5 service segmentation, Chapter 6 medium design, Chapter 7 small design, Chapter 9 firewall/threat inspection, and Chapter 10 Cyber Vision outline | Strong reusable guidance for SD-WAN, zoning, firewall/NGFW/ZBFW/UTD, NAC, Snort, Cyber Vision, and protocol inspection |
| Implementation Guide | Chapter 6 and Chapter 7 platform designs, service VPN mappings, raw socket transport, firewall placement, NAC, Cyber Vision placement | Useful for profile-driven implementation flows and platform-specific design notes |

## Important Scope Notes

- The source document focuses on small and medium transmission substations.
- Large/EHV substations and process bus designs are explicitly out of scope for this 1.0 draft unless added in a later release.
- CVD 3.0 currently includes large, medium, and small substation profiles, so large-substation content should not be sourced only from this document.
- Many figure references are placeholders, such as "Figure x" or "To be filled."
- Product software releases are marked "To be filled" in the source.
- Chapter 9 now contains authored firewall and threat-inspection guidance, including IR8340 NGFW, IR1101 ZBFW, UTD IDS/IPS, DNP3 policy, custom Snort-rule concepts, and event-monitoring guidance.
- Chapter 10, Chapter 11, and appendices remain mostly outlines rather than complete content.
- The updated source contains a numbering conflict: "Chapter 6 Enabling secure access" appears after Chapter 9, while Chapter 6 is also used for Medium Substation Design. The digest preserves this as a source issue.

## Original Chapter Structure

1. Chapter 1 Introduction
2. Chapter 2 Secure Substation Solution Overview
3. Chapter 3 Substation Security Reference Architecture
4. Chapter 4 Security Design Principles
5. Chapter 5 WAN Edge Aggregation and Service Segmentation Architecture
6. Chapter 6 Medium Substation Design
7. Chapter 7 Small Substation Design
8. No Chapter 8 heading is present in the updated source
9. Chapter 9 Firewall and Threat Inspection Design
10. Chapter 6 Enabling Secure Access appears later in the source as a numbering conflict
11. Chapter 10 Cyber Vision Design
12. Chapter 11 Validated Design Summary
13. Appendices for BOM, software matrix, configuration examples, SD-WAN catalogues, policies, acronyms, and references

## Executive Digest

The Secure Substation 1.0 draft defines a Cisco architecture for securing small and medium transmission substations. It combines ruggedized routing, industrial switching, SD-WAN or service VPN segmentation, ESP boundary controls, firewall and inspection functions, Cyber Vision visibility, and legacy serial integration.

The primary design idea is to build a repeatable hub-and-spoke architecture:

- OT data centers host SCADA, control center applications, WAN edge hubs, and operational services.
- SOC services consume Cyber Vision, firewall, authentication, IDS/IPS, and network telemetry.
- NOC services operate Catalyst WAN Manager and WAN lifecycle functions.
- Each substation acts as a spoke with local service segmentation, industrial switching, and WAN edge policy enforcement.

The design separates traffic by function:

- SCADA and station bus
- PMU / synchrophasor
- Physical security
- Workforce enablement
- Management and security operations
- Enterprise or corporate services
- Field area network services
- Legacy serial transport

The security model aligns to NERC CIP and ISA/IEC 62443 principles through:

- Defined Electronic Security Perimeter boundaries
- Zones and conduits
- VRF or service VPN separation
- ESP boundary protection
- WAN encryption
- Integrated or discrete firewall placement
- NGFW, ZBFW, UTD IDS/IPS, and protocol-aware inspection
- NAC using 802.1X and MAB
- Cyber Vision monitoring
- Logging, time synchronization, evidence collection, and failure-mode planning

The updated draft also includes substantially expanded Chapter 9 guidance. That content can now be used directly for CVD firewall and threat-inspection sections rather than being treated only as a future outline.

## Chapter 1: Introduction

The source document includes an outline for Chapter 1 but limited body content.

Planned topics:

- Security concerns in the changing utility landscape
- Grid digitalization
- Substation automation security drivers
- Substation Security 1.0 solution objectives
- Target customers, markets, and deployment environments
- Design guide scope and assumptions
- Standards and regulatory context

### Reuse Guidance For CVD 3.0

Use Chapter 1 as an overview outline. The actual CVD 3.0 overview should expand these themes into a user-facing "why this CVD matters" narrative:

- Substations are shifting from isolated serial environments to connected IP-based OT environments.
- Utilities need segmentation, observability, policy enforcement, and controlled remote operations without disrupting grid reliability.
- CVD 3.0 should explain why Cisco is positioned to combine rugged infrastructure, SD-WAN, security policy, and OT visibility in one repeatable architecture.

## Chapter 2: Secure Substation Solution Overview

Chapter 2 introduces the solution scope, security objectives, deployment profiles, traffic categories, and operational migration considerations.

### Solution Intent

The Cisco Secure Substation solution is intended for utilities modernizing substation communications while preserving reliable grid operations and improving cybersecurity posture.

The solution supports:

- SCADA monitoring and control
- Station bus communications
- PMU / synchrophasor use cases where required
- Physical security services
- Management access
- Workforce enablement
- Integration with control center, SOC, NOC, and utility operations environments

### Solution Objectives

The primary objective is to protect transmission substation communications without disrupting grid operations.

Key objectives:

- Secure aggregation of substation LAN traffic at the WAN edge
- Segmentation using VRFs, VLANs, firewall zones, and service VPNs
- Electronic Security Perimeter boundary protection for protected OT assets
- Support for SCADA and station bus protocols such as DNP3, IEC 60870-5-104, Modbus TCP, and IEC 61850 MMS where applicable
- Legacy serial integration using raw socket or serial pseudowire where required
- Industrial asset visibility and east-west traffic monitoring using Cisco Cyber Vision
- Centralized WAN lifecycle operations using Catalyst WAN Manager, with autonomous operation supported where appropriate
- Alignment with NERC CIP and ISA/IEC 62443 principles

### Security And Compliance Drivers

NERC CIP drives the need to:

- Identify protected cyber assets
- Define Electronic Security Perimeters
- Control routable communications
- Manage remote access
- Monitor security events
- Maintain configuration baselines
- Support evidence-oriented operations

ISA/IEC 62443 complements NERC CIP through:

- Defense-in-depth
- Zones and conduits
- Risk-based segmentation
- Lifecycle security practices for industrial automation and control systems

The solution supports alignment with these frameworks, but compliance depends on the utility's asset classification, regulatory scope, configuration standards, operating procedures, documentation, and audit practices.

### Deployment Profiles

The source document defines small and medium transmission substation deployment profiles.

#### Small Substation Profile

Platform:

- Cisco IR1101

Typical use:

- Compact single-router model for small transmission substations in the 115-230 kV range
- Lower device count and segmentation needs than medium substations
- Still requires secure WAN connectivity, visibility, and controlled access

Typical services:

- SCADA endpoints
- Station bus devices
- Legacy serial RTUs
- Physical security services
- Workforce enablement endpoints
- Management services

Planning values:

| Attribute | Planning value |
| --- | --- |
| Typical point count | 200-600 points |
| Average throughput without PMUs | 50-200 kbps |
| Additional PMU throughput | About 5-10 Mbps per PMU |
| Typical WAN provisioning | 1-10 Mbps over microwave, fiber, or MPLS |
| Common protocols | DNP3, IEC 61850 MMS where applicable, ICCP for tie-lines |

Core capabilities:

- SCADA aggregation
- VRF or service VPN separation
- VLAN segmentation
- Firewall policy
- Legacy serial integration
- Cyber Vision visibility
- Catalyst WAN Manager-based or autonomous operation

#### Medium Substation Profile

Platform:

- Cisco IR8340 Substation WAN Router

Typical use:

- Higher-scale spoke model for medium transmission substations in the 345-500 kV range
- More IEDs, service zones, operational systems, and control center integrations

Typical services:

- Larger station LAN
- SCADA and station bus
- PMU or synchrophasor flows where required
- Physical security systems
- Workforce enablement
- Management services
- Legacy serial connectivity

Planning values:

| Attribute | Planning value |
| --- | --- |
| Typical point count | 500-1,500 points |
| Average throughput without PMUs | 0.5-2 Mbps |
| Additional PMU throughput | About 5-10 Mbps per PMU |
| Typical WAN provisioning | 10-50 Mbps over fiber or high-capacity microwave |
| Common protocols | IEC 61850 MMS, DNP3, ICCP, IEEE C37.118 |

Firewall options:

- Integrated firewall services on the IR8340
- Discrete firewall at the ESP boundary

### Operations And Migration

Many substations include a mix of modern Ethernet-connected devices and legacy serial equipment. The design supports phased migration by allowing legacy RTUs and serial-connected devices to remain operational while the site transitions toward segmented Ethernet/IP architecture.

Operational standardization opportunities:

- WAN connectivity
- Zone mapping
- Service VPNs
- Management access
- Monitoring
- Remote access workflows
- Configuration lifecycle

## Chapter 3: Substation Security Reference Architecture

Chapter 3 defines the reference architecture for small and medium substations. It establishes the major building blocks, logical zones, WAN model, service VPNs, traffic classes, and operational domains used throughout the design.

### Architectural Model

The reference architecture uses a hub-and-spoke model:

- Centralized services reside in OT data centers, SOC, NOC, and utility operations environments.
- Each substation operates as a spoke.
- Each spoke aggregates local traffic, connects to the utility WAN through a WAN edge router, and maps local services into logical zones and service VPNs.
- The same model can be applied across small and medium substations while scaling for device count, WAN transport, and operational requirements.

### Architecture Building Blocks

| Building block | Role |
| --- | --- |
| OT data center | Hosts SCADA, control center applications, data center WAN edge hubs, and OT services |
| Security Operations Center | Hosts or consumes Cyber Vision, security telemetry, event correlation, and incident response workflows |
| Network Operations Center | Hosts Catalyst WAN Manager and WAN control components for onboarding, policy, monitoring, and lifecycle |
| WAN edge routers and transport | Provide hub-and-spoke connectivity between data centers and substations |
| Substation domain | Contains WAN edge router, industrial switches, station bus devices, ESP assets, physical security endpoints, workforce services, management interfaces, and legacy serial devices |

### Platform Roles From Source

| Architecture area | Product or platform | Function in design | Release status |
| --- | --- | --- | --- |
| Headend WAN edge | Cisco Catalyst 8000 Series Edge Platforms | Data center hub routing, WAN aggregation, IPsec termination, substation aggregation | To be filled |
| Medium substation WAN edge | Cisco IR8340 | Substation WAN router, service aggregation, segmentation, serial integration | To be filled |
| Small substation WAN edge | Cisco IR1101 | Compact substation WAN router, service aggregation, segmentation, serial integration | To be filled |
| Medium substation switching | Cisco IE9300 or IE3x00 | Station LAN connectivity, VLAN separation, Cyber Vision sensor support | To be filled |
| Small substation switching | Cisco IE3400 | Compact station LAN connectivity, VLAN separation, Cyber Vision sensor support | To be filled |
| Industrial visibility | Cisco Cyber Vision | Asset discovery, industrial protocol visibility, traffic monitoring | To be filled |
| WAN management | Cisco Catalyst WAN Manager | WAN policy, onboarding, monitoring, lifecycle management | To be filled |
| WAN overlay control | Manager, Controller, Validator | Overlay control, orchestration, secure WAN operation | To be filled |
| Security inspection | Integrated router security or discrete firewall | ESP boundary and zone inspection | To be filled |
| Identity and access | Cisco ISE or approved identity platform | Device and user identity services | To be filled |
| Security operations | SIEM, SOC tools, Cisco security integrations | Event collection, correlation, incident monitoring | To be filled |

### Station Bus, ESP, And Process Bus Scope

The source document primarily describes:

- Station bus
- Electronic Security Perimeter
- Small and medium substation LANs

The process bus is treated as out of scope because large digital substations are not included in the 1.0 design.

Station bus endpoints may include:

- RTUs
- IEDs
- Protection relays
- Gateways
- Bay controllers
- Local HMIs
- Engineering workstations
- SCADA communication devices

Station bus protocols may include:

- DNP3
- IEC 60870-5-104
- Modbus TCP
- IEC 61850 MMS
- SNMP
- Syslog
- SSH
- HTTPS
- File transfer

IEC 61850 GOOSE and Sampled Values are excluded from the validated small and medium architecture in this version.

### Zones And Domains

Typical zones:

- ESP or protected OT zone
- OT SCADA zone
- Station bus zone
- Multiservice zone
- Physical security zone
- Management zone
- Workforce enablement zone
- Corporate services zone
- Field Area Network zone

Design intent:

- Group systems by function, ownership, trust level, and communication requirement.
- Treat conduits between zones as explicit communication paths.
- Map zones and conduits to VLANs, VRFs, service VPNs, firewall zones, and inspection points.

### Traffic And Protocol Profiles

The reference architecture recognizes five major traffic profiles:

| Traffic profile | Description |
| --- | --- |
| SCADA | Monitoring and control between substation and control center; examples include DNP3, IEC 60870-5-104, Modbus TCP |
| PMU | Synchrophasor / wide-area measurement traffic using IEEE C37.118 where deployed |
| Physical security | Video, alarms, access control, door status, badge systems, and related monitoring |
| Enterprise | Approved IT or business-system interaction with substation services |
| Management | Administration, telemetry, logging, backup, authentication, and software lifecycle |

## Chapter 4: Security Design Principles

Chapter 4 defines how the architecture should be protected, monitored, and operated.

### Standards Alignment

NERC CIP alignment is reflected through:

- Defined Electronic Security Perimeter boundaries
- Controlled routable communication into and out of protected zones
- Secure remote access and vendor access handling
- Monitoring of security-relevant events at ESP boundaries and management points
- Segregation of management access from operational control traffic
- Configuration and change-management support through documented baselines
- Logging, time synchronization, and evidence collection

ISA/IEC 62443 alignment is reflected through:

- Zone-and-conduit model
- Function, risk, and trust-level grouping
- Risk-based segmentation
- Firewall, monitoring, identity, and lifecycle controls

### Defense-In-Depth

The defense-in-depth model includes:

- Physical protection of assets, cabinets, cabling, and infrastructure
- Network segmentation using zones, conduits, VRFs, VLANs, and service VPNs
- Boundary protection at ESP and other zone transitions
- Identity-based access for users and devices where supported
- Firewall enforcement and protocol-aware inspection for approved conduits
- Industrial visibility using Cisco Cyber Vision
- Logging, alerting, and time synchronization
- Change control, configuration management, and vulnerability management
- Resiliency and failure-mode planning

### ESP Boundary Protection

ESP boundary controls should provide:

- Known and documented communication paths
- Separation of SCADA, management, physical security, workforce, and enterprise services
- Controlled access from control center systems to approved substation endpoints
- Separation of management interfaces from operational control paths
- Monitoring of traffic entering and leaving protected zones
- Architecture, configuration, and monitoring evidence for operations and audits

### WAN Encryption And Transport

Supported transport types include:

- Fiber
- Microwave
- MPLS
- Carrier Ethernet
- Cellular
- Private circuits
- Internet transport with encryption
- Mixed transport

WAN encryption design should account for:

- Primary and secondary data center paths
- IPsec tunnel scale across substation populations
- Aggregate crypto throughput on Catalyst 8000 Series headend routers
- Substation router platform capacity
- Certificate, key, and lifecycle management
- Failure and reconvergence behavior
- Tunnel state and WAN path monitoring

### Catalyst WAN Manager And Autonomous Operation

Catalyst WAN Manager-based operation provides:

- Centralized onboarding
- Policy distribution
- Monitoring
- Configuration templates
- Lifecycle management

Autonomous operation may be appropriate for:

- Smaller deployments
- Isolated sites
- Staged migrations
- Environments where centralized WAN management is not yet available

Designs should document:

- Device onboarding and identity
- Template and change-control process
- Service VPN and VRF mapping
- Firewall and inspection lifecycle
- Monitoring and alerting
- Software upgrade and rollback
- Behavior during management-plane loss

### Integrated Firewall Vs Discrete Firewall

Integrated firewall model:

- Router provides routing, WAN connectivity, segmentation, and firewall enforcement.
- Lower device count and simpler footprint.
- Well suited to compact small and medium substations.

Discrete firewall model:

- Separate firewall is placed at ESP boundary or between major zones.
- Clearer separation between routing and security functions.
- May offer additional inspection capacity and dedicated security operations ownership.

Selection factors:

- Substation size
- Traffic volume
- Number of zones and conduits
- Required inspection depth
- ESP boundary placement
- Availability and failover requirements
- Space, power, and environmental constraints
- Operational ownership
- Evidence and audit requirements

### NGFW, ZBFW, UTD IDS, And IPS Placement

Inspection placement should consider:

- ESP ingress and egress points
- Control center to substation conduits
- Management and remote access conduits
- Enterprise or corporate service paths
- Physical security service paths
- Cyber Vision monitoring points
- Platform throughput and latency impact

Design guidance:

- Use Zone-Based Firewall to enforce policy between logical zones on the substation WAN edge.
- Use NGFW at boundaries where application visibility, threat inspection, and policy enforcement are required.
- Use IDS where passive detection is preferred.
- Use IPS where signatures, direction, performance, and failure behavior have been validated.
- Stage detection before prevention when baselining industrial protocol behavior.

### SCADA Protocol Inspection And Custom DNP3 Rules

DNP3 inspection should distinguish expected master-to-outstation communication from unexpected attempts.

Custom DNP3 rule examples:

- Allow approved SCADA masters to communicate with known RTUs, gateways, or IED-facing systems.
- Monitor DNP3 write, select, operate, and direct-operate activity.
- Alert on unexpected masters, unexpected outstations, or unexpected directionality.
- Monitor restart, file transfer, time-setting, or configuration-related activity where supported.
- Differentiate normal polling from unusual command frequency or unexpected control actions.
- Apply different handling for monitoring-only and control-capable paths.

IEC 60870-5-104 and Modbus TCP should be handled similarly, with policy reflecting intended source, destination, direction, and system role.

### Network Access Control

NAC design should support:

- 802.1X for endpoints that support supplicant-based authentication
- MAB for fixed-function or legacy OT endpoints that cannot support 802.1X
- Cisco ISE or an approved identity platform
- Static authorization, profiling, port controls, and monitoring

NAC design should define:

- Which device classes use 802.1X
- Which use MAB
- Unknown or unauthorized endpoint handling
- Critical legacy device exemptions
- VLAN, ACL, or group assignment behavior
- Authentication logging and monitoring
- Field replacement and maintenance workflows

### Resiliency And Failure Modes

Failure behavior should be documented for:

- Loss of primary WAN path
- Loss of secondary WAN path
- Loss of Catalyst WAN Manager connectivity
- Loss of control center reachability
- Firewall failure or inspection bypass behavior
- Authentication service outage
- Cyber Vision sensor or monitoring path outage
- Time-source failure
- Power loss and device restart behavior

### Logging, Monitoring, And Time Synchronization

Important log and telemetry sources:

- Firewall events and policy hits
- IDS and IPS events
- Catalyst WAN Manager device and tunnel state
- Router and switch syslog
- Authentication events
- 802.1X and MAB events
- Cyber Vision asset, protocol, and anomaly events
- VPN, VRF, and routing state changes
- Configuration change events
- Time synchronization status

Time synchronization should be defined for:

- Network infrastructure
- Security systems
- Cyber Vision components
- Management platforms
- Logging systems

## Chapter 5: WAN Edge Aggregation And Service Segmentation

Chapter 5 defines the WAN aggregation model for small and medium substations using a hub-and-spoke SD-WAN topology.

### WAN Edge Aggregation

Substation routers act as spokes and terminate at primary and secondary data center hub routers in DC1 and DC2.

Design goals:

- High availability
- Resilient connectivity
- Consistent policy enforcement
- Scalable substation aggregation
- Logical service isolation across the WAN

### Northbound And Southbound Connectivity

Southbound:

- DC hubs connect to WAN infrastructure and aggregate spoke traffic.

Northbound:

- DC hubs connect to corporate, enterprise, security operations, and management environments.
- Dedicated service VPNs can be extended from the data center hubs to application centers.

### Service VPN Model

Baseline service VPN mappings:

| Device / functionality | Service VPN name |
| --- | --- |
| SCADA communication traffic | SCADA-SVC |
| Physical security / surveillance / cameras | PHYSEC-SVC |
| VoIP phone / voice communication | VOIP-SVC |
| Security operations, management, 802.1X / ISE | SEC-OPS-MGMT-SVC |
| User access and workforce enablement | WORKFORCE-SVC |

Connectivity model:

| Device / functionality | Service VPN name | Connectivity model |
| --- | --- | --- |
| SCADA communication traffic | SCADA-SVC | Local service termination |
| Physical security / cameras | PHYSEC-SVC | Extended service connectivity |
| Voice | VOIP-SVC | Extended service connectivity |
| Security operations, management, ISE | SEC-OPS-MGMT-SVC | Extended service connectivity |
| Workforce enablement | WORKFORCE-SVC | Extended service connectivity |

### Service Termination Models

Local service termination:

- Applications are co-located in the data center.
- Example: SCADA application connected locally to the DC hub router and mapped to SCADA-SVC.
- Provides direct, low-latency path for industrial control traffic.

Extended service connectivity:

- Applications are hosted outside the data center.
- Service VPNs extend across the corporate network.
- Used for security operations, management, Cyber Vision, ISE, enterprise applications, physical security, voice, and workforce access.

### TLOC And Tunnel Calculation

Formula:

```text
Total tunnels = spoke TLOCs x hub TLOCs per DC x number of DCs
```

Examples:

| Substation transport | Spoke TLOCs | Hub TLOCs per DC | Total tunnels to DC1 + DC2 |
| --- | --- | --- | --- |
| Dual transport | 2 | 1 | 4 tunnels |
| Single transport | 1 | 1 | 2 tunnels |

Tunnel count is a critical factor for OMP scale and control-plane stability.

### Hub Router Scale Guidance From Source

Example tunnel planning values from the source:

| Hub platform example | Tunnel limit used in source | Dual-transport spoke estimate | Single-transport spoke estimate |
| --- | --- | --- | --- |
| Catalyst 8300 | 6,000 tunnels | Up to 3,000 substations | Up to 6,000 substations |
| Catalyst 8500 | 8,000-10,000 tunnels | Up to 5,000 substations at 10,000 tunnel assumption | Up to 10,000 substations at 10,000 tunnel assumption |

Important source caveat:

- Validate current data sheet limits before using these values.

### Hub Throughput Sizing

Formula:

```text
Total aggregate traffic = average throughput per substation x total number of substations
```

Recommendation:

- Select a hub router with SD-WAN IPsec IMIX throughput that meets or exceeds calculated aggregate traffic.
- Include 20-30% headroom for future growth, bursts, and security overhead.

Source guidance:

- Use Catalyst 8500 Series where aggregate requirement exceeds about 7.8 Gbps IPsec IMIX.
- Use Catalyst 8300 Series where aggregate requirement is below about 7.6 Gbps IPsec IMIX.
- Validate against current data sheets.

### Headend Redundancy

Active/active model:

- Spokes maintain active IPsec tunnels to both DC1 and DC2 hubs.
- Traffic load-balances across both data centers.
- Uses equal OMP cost / ECMP.
- Supports application-aware routing and BFD-driven resiliency.
- Requires symmetry checks where stateful firewalls are in path.

Active/standby model:

- Spokes prefer one hub over the other using TLOC preference or OMP cost.
- Secondary hub remains available as backup.
- Provides deterministic, predictable traffic paths.
- Useful where inspection or audit requirements prefer a primary path.

Comparison:

| Feature | Active/active | Active/standby |
| --- | --- | --- |
| Primary mechanism | Equal OMP cost / ECMP | TLOC preference / OMP cost |
| Bandwidth usage | Aggregated across both hubs | Primary hub only |
| Traffic flow | Dynamic / load-balanced | Deterministic / predictable |
| Complexity | Moderate | Lower |
| Failover | Immediate path-based failover | Immediate control-plane update |

## Chapter 6: Medium Substation Design

Chapter 6 applies the reference architecture and security principles to a medium transmission substation design using IR8340 and industrial Ethernet switching.

### Use Case And Applicability

Validated use cases:

- SCADA monitoring and control between substation and OT data center
- Station bus communication for automation and monitoring
- PMU or synchrophasor traffic where deployed
- Physical security services such as cameras, badge readers, alarms, and monitoring systems
- Controlled workforce enablement
- Management access for routers, switches, firewall functions, Cyber Vision, identity services, logging, and software lifecycle
- Legacy serial integration using RS-232, raw socket transport, or serial pseudowire

### Validated Medium Topology

Core components:

- Single Cisco IR8340 as substation WAN edge
- Cisco IE9320 Industrial Ethernet switching for station LAN
- OT data center SCADA and control center applications
- DC1 and DC2 WAN edge hubs
- SOC for Cyber Vision, firewall, authentication, IDS/IPS, and security telemetry
- NOC for Catalyst WAN Manager and WAN lifecycle functions

### Firewall Placement Options

Integrated firewall:

- IR8340 provides WAN edge routing, service VPN/VRF termination, segmentation, and firewall enforcement.
- Useful for compact designs with fewer devices and simpler cabling.
- IR8340 acts as ESP boundary policy enforcement point.

Discrete firewall:

- IR8340 provides WAN routing and service aggregation.
- Separate firewall enforces policy at ESP boundary or between major service segments.
- Useful where dedicated firewall administration, expanded NGFW inspection, independent policy lifecycle, or SOC ownership is required.

### Single IR8340 Edge Functions

The IR8340 provides:

- WAN edge connectivity to primary and secondary OT data center hubs
- Service aggregation for SCADA, physical security, workforce, management, security operations, PMU, voice, enterprise, and legacy serial traffic
- Service VPN/VRF separation
- VLAN termination or routed handoff from station LAN
- Zone-Based Firewall and integrated security functions where used
- UTD IDS or IPS where supported
- Serial integration using raw socket or serial pseudowire
- Catalyst WAN Manager-based lifecycle operation where centralized management is used

### Medium Service VPN Mapping

| Device or function | Service VPN name |
| --- | --- |
| SCADA communication traffic | SCADA-SVC |
| Physical security, surveillance, cameras, alarms | PHYSEC-SVC |
| Voice and phone services | VOIP-SVC |
| Security operations, management, Cyber Vision, ISE, logging | SEC-OPS-MGMT-SVC |
| Workforce enablement and approved user access | WORKFORCE-SVC |
| PMU or synchrophasor traffic where deployed | PMU-SVC |
| Legacy serial traffic mapped to IP transport | Mapped to SCADA-SVC |

### IE9320 LAN Design

The IE9320 provides rugged station LAN connectivity for:

- IEDs
- RTUs
- Gateways
- Protection relays
- PMU devices
- Engineering workstations
- Physical security endpoints
- Management interfaces
- Cyber Vision sensor functions

LAN design guidance:

- Avoid a flat station network.
- Assign endpoints to VLANs or routed segments by function and communication requirement.
- Keep SCADA and station bus paths tightly scoped.
- Separate physical security from SCADA control.
- Separate management from control traffic.
- Limit VLANs on trunks.
- Apply storm control and multicast controls where required.
- Avoid unmanaged switches inside protected areas.
- Document Cyber Vision monitoring points and east-west visibility.

### Legacy Serial And Raw Socket Transport

Medium substations may include legacy serial RTUs, meters, protection devices, or automation systems.

Design guidance:

- Use raw socket transport to carry serial traffic as IP flows.
- Keep serial SCADA aligned with SCADA-SVC service VPN/VRF.
- Map control center destination, serial interface, source interface, destination address, and TCP/UDP port.
- Tune packetization based on serial protocol behavior using packet length, delimiter, or timeout.
- Use serial pseudowire where circuit-like transport is required by a utility MPLS/SR service.

### NAC For Medium Substations

Use:

- 802.1X for managed endpoints that support supplicants
- MAB for legacy IEDs, RTUs, cameras, badge readers, and embedded controllers
- Cisco ISE or approved identity platform for profiling and authorization

Recommended rollout:

- Start with monitor mode or low-impact mode.
- Validate inventory, 802.1X behavior, MAB behavior, and authentication events.
- Forward events to SOC or approved monitoring platform.

### Cyber Vision For Medium Substations

Cyber Vision should provide visibility into:

- SCADA and station bus communication patterns
- IED, RTU, gateway, and PMU relationships
- Physical security and multiservice traffic where in scope
- Baseline behavior for known assets and protocols
- Deviations from established communication patterns
- Unexpected or unauthorized device communication
- Protocol behavior for incident investigation

Cyber Vision communication should be mapped to the security operations and management service model and separated from SCADA control traffic.

### Medium Traffic Profile

Planning values:

- Typical point count: 500-1,500 points
- Average throughput without PMUs: 0.5-2 Mbps
- Additional PMU throughput: about 5-10 Mbps per PMU
- Typical WAN provisioning: 10-50 Mbps using fiber, MPLS/SR, or Cellular/LTE
- Common protocols: IEC 61850 MMS, DNP3, ICCP, Modbus TCP, IEEE C37.118, HTTPS, SCP, SSH, and serial traffic

Sizing must include:

- Encryption overhead
- Firewall inspection overhead
- Event bursts
- PMU reporting rates
- Video / physical security traffic
- Cyber Vision telemetry
- Software update behavior

## Chapter 7: Small Substation Design

Chapter 7 defines a small secure substation design using IR1101 routing, industrial switching, SD-WAN service VPN segmentation, and integrated firewall policy.

### Small Design Intent

The small substation design is centered on:

- Cisco Catalyst IR1101 as small substation router
- IR1101 as WAN edge and security boundary
- IE3500 in the ESP zone
- IE3100 in the multiservice zone
- IPsec tunnels to data center hub routers
- SD-WAN service VPN segmentation
- Integrated firewall policy on the router

Typical site:

- Usually fewer than 5 devices
- Rarely more than 10 devices

### Core Components

| Component | Functional role |
| --- | --- |
| Cisco IR1101 | Secure WAN edge gateway, routing, ZBFW, IPsec, SD-WAN edge, serial connectivity |
| Cisco IE3500 | ESP zone switch for critical IEDs and utility controller devices; Cyber Vision sensor hosted on this switch |
| Cisco IE3100 | Multiservice zone switch for physical security, VoIP, and workforce access |
| Legacy serial RTUs | Connected directly into IR1101 for serial-to-IP transport |
| IEDs and utility controller devices | Connected through IE3500 ESP switch |
| Cameras, VoIP, user laptops | Connected through IE3100 multiservice switch |
| IRM-1100-4S8I expansion module | Optional port expansion; one port can optionally be used as WAN |

### Northbound Connectivity

The small substation router connects to the WAN using single or dual transport, such as:

- Cellular
- Ethernet
- Optional expansion module port used as WAN

The router acts as an SD-WAN spoke and establishes IPsec tunnels to DC1 and DC2 hub routers. Both active/active and active/standby routing models are supported.

### Southbound Connectivity

Primary LAN zones:

- ESP zone using IE3500 for critical IEDs and protection assets
- Multiservice zone using IE3100 for cameras, workforce mobility, and VoIP
- Switch management delivered inline over a dedicated management VLAN
- Legacy serial devices connected directly to IR1101

### VLAN To Service VPN Mapping

| Device or function | VLAN | Service VPN |
| --- | --- | --- |
| Security operations, Cyber Vision, ISE, switch management, SSH, SNMP, syslog | 100 | SEC-OPS-MGMT-SVC |
| Physical security, surveillance, cameras, alarms | 101 | PHYSEC-SVC |
| Workforce enablement and approved user access | 102 | WORKFORCE-SVC |
| Voice and phone services | 103 | VOIP-SVC |
| SCADA communication, including legacy serial traffic | 201 | SCADA-SVC |

### Small Service Flows

Switch management:

- Uses SEC-OPS-MGMT-SVC
- Carries SSH, SNMP, syslog, and management-plane traffic

Physical security and VoIP:

- Physical security uses PHYSEC-SVC
- Voice uses VOIP-SVC
- Traffic extends from multiservice zone to Enterprise Application Center

Workforce enablement:

1. Endpoint authenticates using 802.1X against ISE over SEC-OPS-MGMT-SVC.
2. After authorization, endpoint is dynamically moved to a VLAN mapped to WORKFORCE-SVC.

SCADA service:

- All SCADA traffic uses SCADA-SVC.
- SCADA includes native IP-based IED traffic and legacy serial RTU traffic tunneled over raw sockets.
- SCADA FEP is architected to communicate only over SCADA-SVC.

### Native IP-Based SCADA Flow

Flow:

1. IED in ESP zone originates DNP3 traffic.
2. IE3500 receives traffic and tags it with SCADA VLAN 201.
3. IR1101 terminates VLAN 201 into SCADA-SVC.
4. IR1101 acts as ESP boundary and applies ZBFW policy.
5. Traffic reaches SCADA FEPs in DC1 and DC2 over the SD-WAN/IPsec overlay.

### Legacy Serial SCADA Via Raw Sockets

Flow:

1. Serial RTU connects directly to IR1101 async serial interface.
2. IR1101 receives serial DNP3 data and encapsulates it into IP using TCP raw sockets.
3. Traffic maps into SCADA-SVC.
4. WAN transport encrypts the flow over IPsec.
5. DC1/DC2 hub routers terminate tunnels and place traffic into SCADA-SVC.
6. SCADA systems receive the raw socket TCP session and deliver serial DNP3 data to SCADA masters.

High availability:

- IR1101 can maintain simultaneous raw socket sessions with SCADA masters in both primary and secondary data centers.
- This reduces single points of failure during legacy serial modernization.

### IR1101 As Integrated Firewall

Example ZBFW roles:

- Stateful DNP3 inspection on TCP port 20000
- Connection initiation controlled from authorized SCADA masters to outstations
- East-west denial between multiservice zone and ESP zone
- Implicit deny for all other ports/protocols targeting SCADA VLAN

Example DNP3 policy model:

- Allow master to outstation: permit TCP/UDP from SCADA master group to substation IED group on port 20000.
- Block outstation initiated TCP SYN to SCADA master unless tightly scoped unsolicited responses are required.
- Drop all other traffic to ESP.

### Small Security And Access Control

The small design uses:

- NAC at the switching layer
- ZBFW at the routing layer
- 802.1X for capable endpoints
- MAB for endpoints without 802.1X support
- Cisco ISE in the Security Operations and Management Center

MAB should be paired with static IP-MAC-port bindings or profiling policies where possible.

### Small Cyber Vision Design

Cyber Vision deployment:

- Sensor hosted as edge compute application on the IE3500 switch inside the ESP
- Monitors north-south communication between IEDs and DC SCADA systems
- Monitors east-west communication between IEDs where in scope
- Communicates with Cyber Vision Center over SEC-OPS-MGMT-SVC

Capabilities:

- DNP3 discovery
- Asset baselining
- Traffic flow analysis
- Anomaly detection

### Small Traffic Profile

Planning values:

- SCADA with 50-200 points: about 10-50 kbps average throughput
- Management: minimal bandwidth for SSH, SNMP, and syslog
- Physical security: 1-3 cameras
- 1080p H.264 at 6-10 fps: about 1-2 Mbps per camera
- 1080p H.264 at 30 fps: about 2-4 Mbps per camera
- Total expected WAN throughput: typically 3-15 Mbps depending on cameras and workforce mobility

## Chapter 9: Firewall And Threat Inspection Design

Chapter 9 now contains authored design guidance for firewall placement, threat inspection, policy modeling, DNP3/Snort concepts, and event monitoring.

### Design Intent

The firewall design should implement the Electronic Security Perimeter as a documented set of Electronic Access Points. Zone-to-zone communication should follow the ISA/IEC 62443 zone-and-conduit model and use a least-privilege, default-deny posture.

Every permitted flow should document:

- Business purpose
- Source zone, destination zone, and address scope
- Protocol, port, and direction
- Authorized initiator
- Inspection requirement
- Enforcement point
- Logging and monitoring expectation
- Rule owner or operational justification

Management, physical-security, workforce, enterprise, and SCADA traffic should remain separated using zones, VRFs, service VPNs, and firewall policy.

### NGFW Design For IR8340 Deployments

The IR8340 can serve as:

- WAN edge
- Service VPN or VRF termination point
- ESP boundary enforcement point
- ZBFW or integrated NGFW platform
- UTD IDS/IPS enforcement point where supported
- Security event and log source

Design guidance:

- Treat service VPNs and VRFs as routing separation, not as complete security policy by themselves.
- Use firewall zones as the explicit policy boundary.
- Create a dedicated zone when a service has a distinct trust level, destination set, inspection requirement, or operational behavior.
- Do not combine SCADA, management, workforce, physical-security, and enterprise services in one zone.
- Pair all route leaking between VRFs with explicit firewall policy.
- Use ZBFW as the baseline enforcement model.
- Apply NGFW, UTD, and SCADA protocol inspection to selected conduits where inspection value justifies processing and failure-domain impact.

### ZBFW Design For IR1101 Deployments

The small substation model uses the IR1101 as the WAN edge and integrated firewall.

Recommended zones:

- ESP-ZONE
- SCADA-ZONE
- SEC-OPS-MGMT-ZONE
- MSP-ZONE

Policy guidance:

- Define directional zone pairs.
- Match traffic using source and destination IP, Layer 4 protocol, application port, and nested matching where required.
- Permit DNP3 only from authorized masters to approved IEDs, RTUs, or gateways.
- Use explicit permits followed by implicit deny.
- Deny routing between multiservice and ESP zones unless a defined conduit requires it.
- Selectively permit SCADA-to-ESP flows.
- Enable logging for important denies and policy hits.
- Default deny toward the ESP.

### Unified Threat Defense IDS/IPS

Use IDS mode first where traffic behavior is not fully baselined or where inline blocking could affect grid operations.

Move to IPS blocking only after validating:

- Traffic direction
- Signatures and false-positive behavior
- Latency and throughput impact
- Resource consumption on the router
- Update and rollback process
- Fail-open, fail-close, or bypass behavior
- Recovery and failback procedures

Use prevention for well-understood threats and keep ambiguous industrial events in detection-only mode until OT and security teams agree that blocking is safe.

### Firewall Policy Model For SCADA And DNP3

SCADA policies should reflect industrial behavior, including long-lived sessions, periodic polling, telemetry bursts, unsolicited events, and control operations.

Every SCADA rule should document:

- Source and destination zones
- Source and destination addresses
- Authorized initiator
- Protocol and port
- Permitted direction
- Inspection action
- Logging action
- IDS, IPS, or pass action
- Owner and justification

DNP3/IP guidance:

- Restrict TCP/UDP 20000 to known masters and known outstations.
- Enforce communication direction.
- Reject unexpected masters, outstations, and directions.
- Separate monitoring-only paths from control-capable paths.
- Avoid any-to-any SCADA-SVC or ESP rules.

Apply the same role, direction, and source/destination discipline to IEC 60870-5-104 and Modbus TCP. Modbus TCP should be especially restrictive because of its limited native security and simple function model.

### IDS/IPS Policy Design

Organize IDS/IPS policy by conduit instead of using one broad policy for all OT traffic.

Priority conduits:

- Control-center-to-substation SCADA
- ESP ingress and egress
- Remote or workforce access paths
- Management conduits
- Enterprise-to-OT paths
- Physical-security paths that cross security boundaries

Policy guidance:

- Separate detection and prevention policy sets.
- Document enabled signatures, disabled signatures, suppressions, thresholds, protected assets, update cadence, rollback approach, and blocking approver.
- Use alert-only policy during baseline collection and commissioning.
- Promote selected signatures to blocking only after operational validation.

### Custom Snort Rules For DNP3

Candidate DNP3 detection use cases:

- Unauthorized master
- Unknown outstation
- Unexpected communication direction
- Write, select, operate, or direct-operate requests on monitoring-only paths
- Unexpected restart, file-transfer, time-setting, or configuration activity
- Unusual control-command frequency
- Departure from the established polling baseline

Rollout guidance:

- Deploy custom rules in alert-only mode first.
- Validate against normal SCADA operations, planned maintenance, failover, and recovery events.
- Coordinate rule promotion with SCADA operations before enabling blocking behavior.

### Integrated Firewall Suitability

Integrated firewall is a strong fit when:

- Site footprint, power, or cabling is constrained.
- Traffic volume and zone count fit the router platform.
- Router and firewall lifecycle can be managed together.
- Centralized templates can maintain policy consistency.
- The shared router/firewall failure domain is acceptable.

This model is suitable for IR1101 small substations and compact IR8340 medium substations.

### Discrete Firewall Insertion

Use a discrete firewall when the design needs:

- Dedicated security administration
- Expanded NGFW capacity or feature depth
- Independent policy lifecycle
- Separate NOC and SOC ownership
- Stronger routing/security separation
- Additional policy granularity

Place the firewall so all applicable routable ESP traffic crosses the enforcement point. Avoid bypass paths and coordinate interfaces, zones, route exchange, service VPN/VRF mapping, and log forwarding as one design.

### Security Event Monitoring

Forward the following event types to the approved monitoring stack:

- Firewall permits, denies, and policy hits
- IDS/IPS alerts and blocking events
- 802.1X and MAB authentication events
- Configuration changes
- Router, switch, and system events
- IPsec, WAN, routing, VRF, and interface state changes
- Cyber Vision asset, protocol, and anomaly events
- Time synchronization failures

All routers, switches, security devices, Cyber Vision components, identity platforms, and log collectors should use trusted, redundant time sources. Consistent time is essential for incident reconstruction, audit evidence, and cross-platform troubleshooting.

### Reuse Guidance For CVD 3.0

Use Chapter 9 as a direct source for the CVD 3.0 NBAR/Snort, firewall, IDS/IPS, DNP3 policy, and event-monitoring sections. Pair it with Chapter 4 security principles and the SD-WAN security policy content.

## Secure Access Outline (Source Numbering Conflict)

The updated source includes "Chapter 6 Enabling secure access" after Chapter 9. This appears to be a numbering conflict because Chapter 6 is also used for Medium Substation Design, and no Chapter 8 heading is present.

This secure-access section is still mostly an outline.

Planned topics:

- SD-WAN fabric overview for substation security
- Catalyst WAN Manager architecture and roles
- Catalyst SD-WAN Manager configuration groups
- Policy groups for NGFW configuration
- Hub-and-spoke topology configuration
- Service VPN design for traffic segregation
- Service provisioning, modification, and removal
- WAN encryption policy
- CLI template requirements and limitations
- Security monitoring for firewall rules and IPS events
- Device management for IR8340 and IR1101
- SD-WAN Manager RBAC for security operations
- SD-WAN configuration catalogue structure

### Reuse Guidance For CVD 3.0

Use this outline to extend the CVD 3.0 implementation guide and SD-WAN Design page. The source does not yet provide enough prose to directly import as full content.

## Chapter 10: Cyber Vision Design

Chapter 10 is an outline in the source document.

Planned topics:

- Cyber Vision architecture for substation security
- Cyber Vision Center placement
- Sensor placement on IE9300, IE3400, IE3x00, and IR platforms
- East-west visibility within the ESP
- North-south visibility for SCADA traffic
- Asset discovery and inventory
- OT asset vulnerability visibility
- Traffic flow analysis
- Baseline creation and change reporting
- Anomaly detection in the ESP
- DNP3 active discovery
- Compliance reporting options
- Cyber Vision deployment guidance by substation model

### Reuse Guidance For CVD 3.0

Use this as a checklist for the Cyber Vision telemetry architecture use case. The current CVD 3.0 architecture already has sensor-to-center telemetry, but Chapter 10 suggests further content on Center placement, asset inventory, baseline reporting, and visibility by substation model.

## Chapter 11: Validated Design Summary

Chapter 11 is an outline in the source document.

Planned summary topics:

- Medium substation integrated firewall design
- Medium substation discrete firewall design
- Small substation integrated firewall design
- SD-WAN headend and hub-and-spoke design
- SCADA monitoring and control traffic
- DNP3-IP, T104, Modbus, HTTPS, SCP, and SSH traffic
- OT-SCADA-VPN traffic segregation
- Physical security service VPN traffic segregation
- Legacy serial transport with raw socket
- Dual data center raw socket connectivity
- ZBFW, NGFW, UTD IDS, and IPS capabilities
- Cyber Vision asset and flow visibility
- Cyber Vision baseline, anomaly detection, and DNP3 active discovery
- NERC CIP-015 visibility considerations

## Appendices Listed In Source

- Appendix A: Product and Software Bill of Materials
- Appendix B: Validated Hardware and Software Matrix
- Appendix C: Configuration Examples
- Appendix D: SD-WAN Configuration Catalogues
- Appendix E: Sample Security Policies
- Appendix G: Acronyms and Glossary
- Appendix H: Reference Documents

## CVD 3.0 Reuse Candidates

### High-Value Reusable Content

- Solution objectives and why-secure-substation narrative
- NERC CIP and ISA/IEC 62443 security drivers
- Small and medium substation profile numbers
- Hub-and-spoke SD-WAN aggregation model
- Service VPN mapping and naming pattern
- Active/active vs active/standby headend model
- ESP boundary protection guidance
- Integrated vs discrete firewall tradeoffs
- DNP3 inspection and custom rule concepts
- Chapter 9 firewall and threat inspection policy model
- IR1101 ZBFW zone model
- IDS-before-IPS rollout guidance
- DNP3 custom Snort candidate detections
- Security event and time-synchronization forwarding list
- NAC with 802.1X and MAB guidance
- Medium IR8340 design details
- Small IR1101 + IE3500/IE3100 design details
- Raw socket guidance for legacy serial SCADA
- Cyber Vision visibility placement and telemetry path

### Gaps To Resolve Before Importing

- Fill validated software releases.
- Confirm current platform names in CVD 3.0, especially IE9300 vs IE9320, IE3400 vs IE3100/IE3500.
- Confirm whether CVD 3.0 should use "Catalyst WAN Manager" or "SD-WAN Manager" naming consistently.
- Validate Catalyst 8300/8500 tunnel scale and throughput numbers against current data sheets.
- Decide whether small-substation profile should remain transmission-focused or align to distribution automation in CVD 3.0.
- Add large-substation content from other sources because this 1.0 draft excludes large/EHV scope.
- Replace placeholder figures with CVD 3.0 diagrams or remove figure references.
- Resolve the source chapter numbering conflict: Chapter 6 appears twice and no Chapter 8 heading is present.
- Complete the secure access outline, Chapter 10, Chapter 11, and appendices if they are needed as final user-facing content.

## Suggested CVD 3.0 Integration Plan

1. Use Chapter 2 to strengthen the Overview page with solution intent, target audience, compliance drivers, and migration narrative.
2. Use Chapter 3 and Chapter 5 to enrich the Architecture section with hub-and-spoke topology, service VPNs, zones, conduits, and traffic profile language.
3. Use Chapter 4 to deepen Design Guide content around ESP boundary protection, SD-WAN operation, firewall models, inspection placement, NAC, resiliency, and logging.
4. Use Chapter 6 and Chapter 7 to improve the Implementation Guide for medium and small profile topology flows.
5. Use Chapter 9 as direct input for NBAR/Snort/firewall, IDS/IPS, DNP3 policy, and event-monitoring design.
6. Use the secure-access outline to extend the SD-WAN Design page and future Catalyst WAN Manager implementation content.
7. Use the Chapter 10 outline as a placeholder for Cyber Vision design expansion.
8. Keep this digest as source-trace context rather than final polished copy.
