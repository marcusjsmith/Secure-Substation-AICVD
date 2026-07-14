# Secure Substation CVD Content

Markdown companion for the Secure Substation AICVD project.

Source project files:

- `secure substation diagram-static-flows.html`
- `architecture-diagram-cybervision 1.1.html`
- `design.md`
- `README.md`

Live project page:

- https://marcusjsmith.github.io/Secure-Substation-AICVD/secure%20substation%20diagram-static-flows.html

This file captures the project content in a portable Markdown format. It mirrors the current CVD navigation model: Overview, Architecture, Design Guide, and Implementation Guide.

## Navigation Structure

The CVD is organized into four primary menu areas:

1. Overview
2. Architecture
3. Design Guide
4. Implementation Guide

Architecture contains two sub pages:

- Diagram / Use Cases
- Traffic Profiles

Design Guide contains three sub pages:

- SD-WAN Design
- NERC CIP Zoning
- NBAR / Snort Policy

Implementation Guide presents the deployment as profile-driven topology flows for large, medium, and small substations. Product boxes link to configuration content for the related Cisco platforms.

## Overview

### Secure Substation CVD Overview

The Secure Substation CVD is a Cisco Validated Design reference for building secure, segmented, and observable utility substations across large transmission, medium distribution, and compact field-site profiles.

The design brings together ruggedized routing and switching, Cisco SD-WAN segmentation, Cisco Cyber Vision visibility, and policy-driven security controls into a repeatable substation blueprint. It is organized around the network behaviors utilities need to preserve:

- Deterministic SCADA reachability
- Resilient WAN transport
- Bounded management access
- Monitored OT assets
- Controlled paths for higher-bandwidth services such as PMU and physical-security video

The design uses three deployment tiers so the same principles can be applied without overbuilding smaller sites:

- Large substations with dual-router resilience
- Medium substations with a single rugged WAN edge
- Small substations using compact IR1101-based routing

Each tier ties back to service VPN separation, WAN sizing, Cyber Vision telemetry, and security policy placement.

### Why Read This CVD

Substations are no longer isolated serial environments. Modern utility sites carry SCADA, management, telemetry, PMU, video, and remote-support traffic across shared IP infrastructure while still needing deterministic operations, cyber separation, and auditable security controls.

This CVD gives engineering, OT operations, and security teams a common reference for turning those requirements into a repeatable architecture. It shows how to structure the substation edge, how to separate critical services, where to apply inspection, and how to size the WAN for large, medium, and small sites.

Key reasons to read the CVD:

- **Reduce design ambiguity:** Align network, OT, and security teams around one validated blueprint instead of one-off substation builds.
- **Protect critical operations:** Preserve SCADA reachability while adding segmentation, firewall policy, application recognition, and inspection where it matters.
- **Plan before deployment:** Relate traffic types, bandwidth assumptions, and product choices before configuration begins.
- **Improve audit readiness:** Map architecture decisions to zoning, access control, monitoring, and operational evidence expected in regulated utility environments.

### Why Cisco

Cisco brings the substation WAN, industrial LAN, security policy, and OT visibility layers into one architecture. Secure substations need more than a router, a switch, or an IPS rule in isolation. They need those controls to work together across the full lifecycle.

Cisco capabilities used in this CVD:

- **Rugged OT infrastructure:** Industrial routers and switches such as IR8340, IR1101, IE9300, IE3100, and IE3500 support hardened deployment models for utility environments.
- **SD-WAN and segmentation:** Cisco SD-WAN provides centralized overlay policy, service VPN separation, encrypted transport, and scalable operations for many substations.
- **Security policy at the edge:** Zone firewall, application-aware policy, NBAR, and Snort IDS/IPS capabilities can be aligned to the same traffic paths shown in the architecture.
- **OT visibility:** Cisco Cyber Vision adds asset, protocol, and anomaly context so policy decisions are informed by what is actually running in the substation.

### What The CVD Establishes

The CVD establishes a repeatable design model for:

- **Service separation:** SCADA, MultiService, and Management traffic are treated as distinct service VPNs with different trust, QoS, and inspection requirements.
- **Substation profiles:** Large, medium, and small designs map platform choices to expected device scale, traffic volume, and resiliency needs.
- **Operational visibility:** Cyber Vision sensors and center connectivity provide asset discovery, protocol awareness, and anomaly context across OT zones.
- **Security alignment:** NERC CIP impact categories, NGFW policy, NBAR application recognition, and Snort IPS are positioned against the architecture.

### Design Outcomes

The intended outcomes are:

- **Standardize the substation edge:** Use a common routing, switching, and SD-WAN pattern while still sizing the design for large, medium, and small facilities.
- **Protect critical OT paths:** Constrain SCADA, management, and multiservice flows with segmentation, zone-based firewalling, application matching, and IPS inspection where required.
- **Plan WAN capacity:** Relate traffic types such as DNP3, PMU, CCTV, enterprise access, and management telemetry to practical bandwidth and QoS assumptions.
- **Connect design to deployment:** Carry the same architecture into platform references and configuration examples for routers, switches, and security functions.

### Guide Sections

| Section | Purpose | Content |
| --- | --- | --- |
| Overview | Frames the CVD purpose, operating model, deployment tiers, and relationship between architecture, controls, and implementation artifacts. | Scope, audience, outcomes, assumptions, section flow |
| Architecture | Shows the base substation topology, selectable traffic-flow overlays, Cyber Vision telemetry paths, and bandwidth model for each site profile. | SCADA, management, MultiService, controller, IPsec, Cyber Vision, traffic planning |
| Design Guide | Maps the architecture to security policy decisions. | SD-WAN, NERC CIP impact context, Snort IPS, NBAR, NGFW policy |
| Implementation Guide | Translates the validated design into product references and configuration patterns. | IR8340, IR1101, IE9300, IE3100, IE3500 topology and configuration steps |

## Architecture

### Diagram / Use Cases

The architecture page provides the core topology with selectable overlays for:

- SCADA traffic
- Management traffic
- MultiService traffic
- Controller overlay traffic
- IPsec hub-and-spoke traffic
- Cyber Vision telemetry architecture

The base architecture includes:

- SD-WAN fabric
- Control-center headends
- Cisco SD-WAN controllers
- Large substation zone
- Medium substation zone
- Small substation zone
- Rugged WAN edge routers
- Industrial aggregation and access switching
- Cyber Vision Center and embedded sensors

### SCADA Traffic

SCADA traffic represents OT supervisory communication between control-center masters and substation RTUs or IEDs. It is carried on the segregated SCADA service VPN.

Primary example:

- **DNP3:** TCP/UDP, commonly port 20000, used for polling and unsolicited responses from masters to field devices.

Design intent:

- Keep SCADA separated from management and multiservice traffic.
- Permit only approved control-center masters and substation endpoints.
- Apply zone-based firewall policy at the WAN edge.
- Use QoS to preserve supervisory traffic during congestion.
- Apply NBAR and Snort where application classification and protocol inspection are required.

### Management Traffic

Management traffic covers operations, monitoring, and administration between NMS, jump hosts, and field infrastructure. It is typically modest in bandwidth but important for visibility and secure remote support.

Example protocols:

- **SSH:** Interactive CLI and scripted automation to routers, switches, and appliances.
- **HTTPS:** Web UI, REST APIs, and certificate-based management sessions.
- **Syslog:** Centralized event and alarm forwarding for correlation and SIEM ingestion.
- **NetFlow / IPFIX:** Sampled flow records exported to collectors for capacity planning and troubleshooting.
- **SNMP:** Poll-based metrics and traps for fault, performance, and inventory monitoring.

Design intent:

- Keep management access on dedicated paths.
- Restrict administrative access to approved jump hosts and operations systems.
- Log and monitor all management access.
- Keep direct WAN-to-switch management blocked unless explicitly required and controlled.

### MultiService Traffic

MultiService traffic covers higher-bandwidth or non-SCADA OT workloads. It often becomes the dominant WAN consumer where PMUs and cameras are deployed.

Example workloads:

- **CCTV:** IP camera video using RTSP/RTP and H.264/H.265 for physical security.
- **PMU:** IEEE C37.118 synchrophasor UDP/TCP streams at 30-60 samples per second. Per-PMU throughput is commonly 1-5 Mbps.
- **AMI data:** Advanced metering infrastructure backhaul from collectors or gateways to head-end systems.

Design intent:

- Keep high-bandwidth traffic out of the SCADA service VPN.
- Apply QoS so PMU or video traffic cannot starve SCADA.
- Use bandwidth planning to size WAN headroom.
- Use NBAR visibility to confirm what applications traverse each service VPN.

### Controller Overlay Traffic

Controller overlay traffic represents outbound control-plane sessions from every SD-WAN edge router to Cisco SD-WAN orchestration and validation services.

Example control-plane traffic:

- **OMP:** Overlay Management Protocol over DTLS between SD-WAN edge routers and controllers for route, policy, and tunnel state.
- **HTTPS / NETCONF:** Manager and Validator reachability, template deployment, and certificate lifecycle.
- **DTLS/TLS:** Encrypted control channels for SD-WAN control, not user or OT application payload.

Design intent:

- Treat controller reachability as required for SD-WAN fabric operation.
- Keep OT payload policy separate from controller-plane reachability.
- Validate controller identity and certificate processes during onboarding.

### IPsec Hub And Spoke

The IPsec hub-and-spoke overlay provides encrypted connectivity between substation spoke routers and control-center headend hubs.

Example traffic:

- **IKEv2:** Key exchange and peer authentication between IR8340/IR1101 spokes and IR8340 headends.
- **ESP using AES-GCM:** Encrypted encapsulation according to utility crypto policy.
- **SD-WAN IPsec:** Tunnel interfaces bound to SD-WAN transport templates, with hubs terminating spoke overlays for routing and service VPN handoff.

Design intent:

- Encrypt WAN paths between substations and headends.
- Use hub-and-spoke topology where central inspection or routing control is required.
- Align tunnel configuration with SD-WAN templates and utility cryptographic standards.

### Cyber Vision Telemetry Architecture

Cyber Vision telemetry is presented as an architecture use case. Embedded Cyber Vision sensors on IE9300 and IE3x00 switches send DPI-derived metadata and security events to the Cyber Vision Center over the SD-WAN fabric.

Direction:

- Substation sensor -> control center Cyber Vision Center

Primary port and service requirements:

| Service | Protocol / Port | Role |
| --- | --- | --- |
| Sensor telemetry and policy | TCP 443, TLS/HTTPS | Primary channel for asset inventory, flow metadata, events, and certificate-pinned communication to the Center |
| DNS | UDP/TCP 53 | Optional name resolution for Cyber Vision Center FQDN |
| NTP | UDP 123 | Optional time synchronization per utility policy |

Example paths:

- Large site: IE9300 sensor -> IR8340 -> SD-WAN fabric -> control-center firewall/NMS -> Cyber Vision Center
- Small site: IE3x00 sensor -> IR1101 -> hub -> Cyber Vision Center

Cyber Vision DPI observes OT locally through SPAN, embedded tap, or sensor capability. It does not replace SCADA protocols.

### Traffic Profiles

Traffic profile planning supports service VPN segregation, QoS, and WAN sizing for:

- SCADA using DNP3
- PMU synchrophasors
- Physical security video
- Enterprise access
- Management plane traffic

Planning values are representative and should be validated against each utility's scan lists, PMU counts, camera codec settings, and operational requirements.

#### Total Bandwidth By Substation Type

| Substation type | Platform | Typical site | Low scenario | High scenario |
| --- | --- | --- | --- | --- |
| Small | Single IR1101 + IE3100/IE3500 | Distribution automation or field site, usually fewer than 10 devices | ~1.01 Mbps | ~12.05 Mbps |
| Medium | Single IR8340 + dual IE9320 | Smaller transmission or primary distribution, typically 10-15 IEDs and RTUs | ~10.1 Mbps | ~60.2 Mbps |
| Large | Dual IR8340 + dual IE9320 in HA | Large transmission or EHV, generally 15+ IEDs plus PMU, fault recorders, and RTUs | ~15.2 Mbps | ~81 Mbps |

#### Large Substation

Platform:

- Dual IR8340 + dual IE9320 in high availability
- DNP3 SCADA
- Zone-based firewall
- SD-WAN catalogue deployment

Typical site:

- Large transmission or EHV
- Generally 15+ IEDs plus PMU, fault recorders, and RTUs

Traffic profile:

| Traffic type | Bandwidth / notes |
| --- | --- |
| SCADA (DNP3) | Large EHV substation with more than 1,000 points: approximately 200 kbps to 1 Mbps |
| PMU (C37.118) | 30-60 samples per second per PMU, 1-5 Mbps per stream, 2-4 PMUs can produce about 5-20 Mbps aggregate |
| Physical security | 10-15 cameras. 1080p H.264 at 6-10 fps is typically 1-2 Mbps per camera; at 30 fps, typically 2-4 Mbps per camera |
| Enterprise | Site-dependent workforce mobility traffic |
| Management | SSH, SNMP, syslog, and related management traffic |

Indicative total:

- ~15-81 Mbps
- Low scenario: 200 kbps SCADA, 5 Mbps PMU aggregate, 10 cameras at ~1 Mbps
- High scenario: 1 Mbps SCADA, 20 Mbps PMU aggregate, 15 cameras at ~4 Mbps

#### Medium Substation

Platform:

- Single IR8340 + dual IE9320
- DNP3 SCADA
- Zone-based firewall
- SD-WAN catalogue deployment

Typical site:

- Smaller transmission or primary distribution
- Typically 10-15 IEDs and RTUs

Traffic profile:

| Traffic type | Bandwidth / notes |
| --- | --- |
| SCADA (DNP3) | Medium substation with 200-1,000 points: approximately 50-200 kbps |
| PMU (C37.118) | Same per-PMU characteristics as large: 30-60 samples per second, 1-5 Mbps per stream, 2-4 PMUs often 5-20 Mbps combined |
| Physical security | 5-10 cameras. 1080p H.264 at 6-10 fps is typically 1-2 Mbps per camera; at 30 fps, typically 2-4 Mbps per camera |
| Enterprise | Workforce mobility traffic |
| Management | SSH, SNMP, syslog |

Indicative total:

- ~10-60 Mbps
- Low scenario: 50 kbps SCADA, 5 Mbps PMU aggregate, 5 cameras at ~1 Mbps
- High scenario: 200 kbps SCADA, 20 Mbps PMU aggregate, 10 cameras at ~4 Mbps

#### Small Substation

Platform:

- Single IR1101 + IE3100/IE3500 access switches
- DNP3 SCADA
- Zone-based firewall
- Compact SD-WAN / distribution automation profile

Typical site:

- Distribution automation or field site
- Usually fewer than 10 devices and commonly fewer than 5

Traffic profile:

| Traffic type | Bandwidth / notes |
| --- | --- |
| SCADA (DNP3) | Small substation with 50-200 points: approximately 10-50 kbps average |
| Management | SSH, SNMP, syslog |
| Physical security | Optional video with 1-3 cameras. 1080p H.264 at 6-10 fps is typically 1-2 Mbps per camera; at 30 fps, typically 2-4 Mbps per camera |

Indicative total:

- ~1-12 Mbps
- Low scenario: 10 kbps SCADA plus one camera at ~1 Mbps
- High scenario: 50 kbps SCADA plus three cameras at ~4 Mbps
- No PMU included in this profile

#### Planning Notes

- **PMU dominance:** Where PMUs are deployed, synchrophasor UDP/TCP streams often drive WAN headroom ahead of DNP3/MMS SCADA.
- **Video variability:** Actual camera bitrate depends on resolution, codec, FPS, scene motion, and whether streams are continuous or event-triggered.
- **Service VPNs:** The SRD assumes segregated service VPNs or equivalent VRF/zone paths for SCADA, physical security, enterprise, management, and PMU where applicable.
- **Interactive diagram:** The topology view maps these tiers to dual IR8340 for large transmission, single IR8340 for medium distribution, and IR1101 for small distribution sites.

## Design Guide

### SD-WAN Design

The SD-WAN design positions Cisco Catalyst SD-WAN in the Secure Substation CVD. It covers controller roles, overlay construction, service VPN segmentation, and security policy capabilities applied at the rugged WAN edge.

The SD-WAN design creates a secure overlay between substation routers and control-center headends while keeping SCADA, Management, and MultiService traffic separated. IR8340 and IR1101 routers act as substation WAN edges. The controller stack supplies orchestration, identity, route exchange, and central policy.

#### Architecture Model

- **Transport independence:** The SD-WAN overlay runs across MPLS, private fiber, broadband, LTE, or mixed utility WAN transports. Each router authenticates into the fabric and builds encrypted tunnels to approved peers.
- **Service segmentation:** OT and operations traffic is mapped into separate service VPNs or VRFs. SCADA remains tightly constrained, Management is limited to approved operations systems, and MultiService carries higher-bandwidth workloads such as PMU and physical-security video.
- **Substation fit:** Large substations use resilient IR8340 edge pairs, medium substations use a single IR8340 edge, and small substations use IR1101 with IE3100/IE3500 access switching.

Service VPN examples:

- **SCADA service VPN:** DNP3, IEC 61850 MMS, RTU/IED supervision, and control-center master traffic with strict zone firewall and QoS treatment.
- **Management service VPN:** Device administration, telemetry, syslog, SNMP, NetFlow/IPFIX, certificate operations, and remote support paths.
- **MultiService VPN:** PMU, CCTV, AMI, and approved utility applications that need bandwidth without collapsing into the SCADA zone.

#### SD-WAN Controller Roles

| Controller role | Previous naming | Plane | Purpose |
| --- | --- | --- | --- |
| SD-WAN Manager | vManage | Management plane | Central GUI and API for device onboarding, templates, policy design, monitoring, troubleshooting, and software lifecycle |
| SD-WAN Validator | vBond | Identity plane | Authenticates devices and controllers during onboarding and helps edge routers discover the controller fabric |
| SD-WAN Controller | vSmart | Control plane | Maintains secure control sessions, exchanges overlay routing using OMP, and distributes centralized policy |

SD-WAN Manager capabilities:

- Creates and distributes feature templates for IR8340 and IR1101 routers.
- Manages VPNs, interface roles, routing, QoS, and security policy.
- Provides operational views for tunnel health, alarms, application visibility, policy counters, and events.

SD-WAN Validator capabilities:

- Authenticates devices and controllers during onboarding.
- Supports certificate-based identity and serial/chassis validation.
- Reduces the risk of unapproved edge devices joining substation services.

SD-WAN Controller capabilities:

- Maintains secure control sessions to WAN edge routers.
- Exchanges overlay routing information using OMP.
- Distributes policies for segmentation, path selection, route reachability, and tunnel behavior.

#### SD-WAN Security Policy Capabilities

Security policy in the CVD includes service VPN segmentation, zone-based firewall/NGFW policy, Snort IDS/IPS, and NBAR application-aware policy.

##### Service VPN And Zone Design

Role:

- Keep substation workloads separate before inspection policy is applied.

Pattern:

- SCADA VPN to SCADA zone
- Management VPN to management zone
- MultiService VPN to video, PMU, and application zone
- Explicit inter-zone rules between all zones

Controls:

- Restrict SCADA flows to known masters, RTUs, and approved protocols.
- Keep management access on dedicated paths with MFA and jump-host controls upstream.
- Apply QoS and path policy per service so PMU or video traffic cannot starve SCADA supervision.

##### Zone-Based Firewall And NGFW Policy

Role:

- Enforce permit, deny, log, and inspect decisions between WAN, SCADA, management, and multiservice zones.

Pattern:

- WAN SCADA zone to substation SCADA zone permits DNP3 only from approved masters to approved RTUs.
- All other traffic is denied and logged.

Controls:

- Permit only approved source and destination pairs for DNP3, HTTPS, SSH, syslog, SNMP, NetFlow/IPFIX, PMU, and video flows.
- Deny and log unknown or cross-zone traffic that does not match the substation communication baseline.
- Use application-aware matches where supported.

##### Snort Intrusion Detection And Prevention

Role:

- Inspect selected flows beyond 5-tuple firewall rules and detect or block protocol misuse.

Pattern:

- Permit DNP3 between known endpoints, then inspect with Snort to alert or drop malformed frames, unexpected function codes, or traffic outside the approved master-outstation pattern.

Controls:

- Detect suspicious activity in approved flows such as DNP3, Modbus/TCP, IEC 61850 MMS, HTTPS, or management traffic.
- Run in alert/monitor posture during tuning.
- Move selected controls to prevention where false positives are understood.
- Use custom OT-focused rules when utility-specific SCADA behavior needs tighter validation.

##### NBAR And Application-Aware Policy

Role:

- Classify applications so firewall, QoS, and inspection decisions can use application identity.

Pattern:

- Application = DNP3
- Source = control-center master subnet
- Destination = substation RTU subnet
- Action = allow plus inspect

Controls:

- Match application names or families in policy sequences where supported by platform and release.
- Align application recognition with QoS so SCADA and PMU receive predictable treatment during WAN congestion.
- Use visibility data to validate traffic profiles documented in the Architecture section.

#### SD-WAN Design Guardrails

- **Validate release support:** Security features vary by IOS XE SD-WAN release, platform, and license. Confirm IR8340 and IR1101 support before final policy baselines.
- **Separate control and data planes:** Controller connectivity is required for overlay operation, but OT payload policy should be designed per service VPN and zone boundary.
- **Use staged enforcement:** Build policy from visibility first, then move from monitor/log to enforce/drop on carefully scoped OT paths.
- **Coordinate with Cyber Vision:** Use Cyber Vision asset and protocol visibility to refine SD-WAN application, firewall, and IDS/IPS rules.

#### SD-WAN Reference Links

- [Cisco Catalyst SD-WAN overview](https://www.cisco.com/c/en/us/solutions/enterprise-networks/sd-wan/index.html)
- [Cisco Catalyst SD-WAN configuration guides](https://www.cisco.com/c/en/us/support/routers/sd-wan/products-installation-and-configuration-guides-list.html)
- [Cisco IOS XE SD-WAN security configuration guide](https://www.cisco.com/c/en/us/td/docs/routers/sdwan/configuration/security/ios-xe-17/security-book-xe.html)

### NERC CIP Zoning

The NERC CIP zoning page uses the architecture diagram to highlight Medium Impact, Low Impact, and Non-BES substation zones before applying the SD-WAN security policy model.

| Impact category | Definition | Diagram mapping |
| --- | --- | --- |
| Medium Impact | BES Cyber Systems where failure or compromise within 15 minutes could adversely affect reliable BES operation | Large and Medium substation zones |
| Low Impact | In-scope BES Cyber Systems with lower reliability consequence; CIP requirements are present but typically less stringent than Medium Impact | Medium and Small substation zones |
| Non-BES | Facilities and cyber assets outside the Bulk Electric System, commonly distribution automation and field locations not designated as BES | Small substation zone only |

Design interpretation:

- Large and medium substations are the primary zones for Medium Impact consideration.
- Medium and small substations may map to Low Impact depending on registration and reliability function.
- Compact distribution automation or field sites may map to Non-BES where they are outside BES scope.

The CVD uses the zoning view to connect topology, product selection, service segmentation, and security controls to the likely regulatory context.

### NBAR / Snort Policy Design

The NBAR / Snort policy page applies application-aware classification and deep packet inspection after the zoning model is understood. It shows how NBAR, ZBFW, NGFW policy, and Snort IPS work together on the SD-WAN edge.

The Secure Substation CVD uses application-aware security on Cisco SD-WAN edge routers such as IR8340 and IR1101:

- **NBAR** identifies OT and IT protocols so policies can match traffic precisely.
- **Custom Snort IPS rules** perform deep protocol inspection, which is important for SCADA protocols such as DNP3 that generic port-based ACLs cannot fully protect.

Context:

- Zone-based firewall and service VPN segregation define trust boundaries.
- Within and between those zones, NGFW features on IOS XE SD-WAN combine NBAR classification with Snort inspection and centrally managed security policy.
- This aligns to NERC CIP electronic access and monitoring expectations without blocking legitimate OT operations.

#### Packet Flow Through SD-WAN Edge Router

Representative northbound SCADA path:

- DNP3 master -> substation RTU over the SCADA service VPN

Packet flow:

1. Ingress packet enters the WAN or tunnel interface.
2. Packet is assigned to the source security zone.
3. NBAR classifies the application, such as DNP3.
4. ZBFW / NGFW policy evaluates the zone-pair rule.
5. If the policy action is Permit or Pass, the packet is allowed through and bypasses Snort.
6. If the policy action is Deny, the packet is dropped and logged.
7. If the policy action is Inspect, the flow is handed to Snort IPS for protocol inspection.
8. On Snort pass, traffic continues to SD-WAN/QoS and egress.
9. On IPS match, traffic is dropped and logged.

#### Custom Snort Rules

Role:

- Protocol-level intrusion prevention and anomaly detection on traffic selected by security policy, not only allow/deny by port.

Snort runs as an IPS service on the SD-WAN router through UTD/Snort integration on supported IOS XE releases. Custom rule sets extend the stock signature base so utilities can enforce OT-specific expectations:

- Valid DNP3 function codes
- Permitted master-outstation relationships
- Detection of malformed or unauthorized application payloads

Use cases:

- **DNP3:** Inspect TCP/UDP traffic, commonly port 20000, for protocol violations, unexpected function codes, or traffic that does not conform to the utility SCADA profile.
- **Other OT protocols:** Modbus/TCP, IEC 61850 MMS/GOOSE where supported, and IEC C37.118 PMU can be targeted with tailored rules where inspection is required on WAN or zone boundaries.
- **Policy actions:** Alert, drop, or rate-limit per SD-WAN security policy.
- **Operations:** Test custom rules in monitor-only mode first and tune false positives against known master scan schedules and unsolicited responses.

Example:

- On the SCADA service VPN, permit DNP3 between approved control-center and substation RTU subnets, but use Snort to alert or block DNP3 frames with disallowed function codes or anomalous lengths.

#### NBAR Application Recognition

Role:

- Identify applications and protocols in flow traffic so SD-WAN security policies can match on application name, not only IP, port, or protocol number.

NBAR performs stateful deep packet inspection to classify traffic into application categories. On SD-WAN edges, NBAR attributes are used by the NGFW or unified security policy engine to build rules such as:

- Permit DNP3
- Deny unknown
- Inspect Modbus

Capabilities:

- **OT protocol matching:** NBAR includes industrial and SCADA-related classifiers such as DNP3, Modbus, IEC 61850 MMS, BACnet, and others depending on IOS XE/SD-WAN release.
- **NGFW integration:** Security policy sequences can match application or application-family and apply firewall, IPS, or other actions.
- **QoS alignment:** The same classification can drive QoS marking for DNP3 and C37.118 PMU streams.
- **Visibility:** Application visibility in SD-WAN management and NetFlow/IPFIX exports supports capacity planning and traffic validation.

Example policy matches:

- Match: Application = DNP3; source = SCADA master zone; destination = substation RTU zone; action = Inspect with Snort and allow.
- Match: Application = unknown; VPN = SCADA; action = deny and log.

#### How Snort And NBAR Work Together

In the Secure Substation architecture:

- NBAR is the classifier.
- Snort is the inspector.
- ZBFW / NGFW policy decides whether to permit, deny, or inspect.

Design pattern:

1. Segregate traffic into SCADA, MultiService, and Management service VPNs.
2. Use ZBFW zone pairs per design requirements.
3. Apply centrally managed NGFW policy with NBAR application matches.
4. Enable Snort IPS, including custom OT rules, on high-risk zone pairs or WAN boundaries.

Example DNP3 flow:

1. Classify with NBAR as DNP3.
2. Permit only between approved zones.
3. Validate protocol behavior with Snort.
4. Preserve supervisory traffic with QoS during congestion.

Deployment note:

- Confirm IOS XE SD-WAN release, UTD/Snort licensing, and NBAR classifier support for the exact OT protocol mix on IR8340 and IR1101 before finalizing baselines.
- Treat custom Snort rules like any other BES cyber system change where CIP change management applies.

#### Related CVD Controls

- **Zone-based firewall:** On-router ZBFW between SCADA, process, and WAN zones.
- **Service VPNs:** SCADA, MultiService, and Management VRFs/VPNs limit where NGFW policies must be applied.
- **Traffic flows view:** SCADA and management overlays visualize the paths protected by security policy.

## Implementation Guide

### Substation Implementation Flows

The implementation guide starts from the substation profile, then follows the basic topology from rugged WAN edge to industrial switching. Each product box opens the relevant configuration sequence so the guide reads as a deployment flow instead of a datasheet list.

### Large Substation: Transmission / EHV Site

Use redundant IR8340 routers for WAN and SD-WAN edge resilience, with IE9300 switches providing rugged aggregation for SCADA, PMU, engineering access, video, and Cyber Vision sensor coverage.

Topology relationship:

1. **WAN edge and SD-WAN HA**
   - IR8340 primary: Primary SD-WAN edge, zone firewall, IPsec transport, and service VPN handoff.
   - IR8340 secondary: Redundant edge for large-site survivability, LTE/fiber backup, and policy continuity.
2. **Routed trunks and segmented service VPNs**
3. **Industrial aggregation**
   - IE9300 aggregation A: SCADA, PMU, station bus, and engineering VLAN aggregation with rugged timing support.
   - IE9300 aggregation B: Redundant industrial aggregation and Cyber Vision sensor point for mirrored OT visibility.

Implementation notes:

- Use for high-availability transmission substations with multiple protection, control, and monitoring zones.
- Separate SCADA, management, multiservice, and Cyber Vision telemetry using routed interfaces or VLAN trunks.

### Medium Substation: Distribution / Sub-Transmission Site

Use a single IR8340 WAN edge where rack space and uplink count are moderate, paired with IE9300 aggregation for rugged Layer 2/Layer 3 switching and embedded Cyber Vision telemetry.

Topology relationship:

1. **WAN edge**
   - IR8340: Primary substation router for SD-WAN, firewall segmentation, IPsec, and optional cellular backup.
2. **Service handoff to industrial switching**
3. **Aggregation / access**
   - IE9300 core: Rugged aggregation for protection, control, management, and sensor telemetry VLANs.
   - IE3100 access: Optional DIN-rail access layer for smaller IED clusters, RTUs, meters, or local cameras.

Implementation notes:

- Use when the substation needs strong segmentation but does not require dual-router edge HA.
- IE3100 can extend access from the IE9300 when field panels or remote bays need hardened Ethernet.

### Small Substation: Compact Distribution / DA Site

Use IR1101 as the compact routed edge, with IE3100 or IE3500 switching underneath depending on port density, PoE, uplink speed, and local service requirements.

Topology relationship:

1. **Compact WAN edge**
   - IR1101: Small-site SD-WAN router for Ethernet or LTE WAN, zone firewall, and routed LAN handoff.
2. **Routed LAN handoff to DIN-rail switching**
3. **Industrial access**
   - IE3100: Compact rugged access for small IED groups, RTU connections, and low-port-count bays.
   - IE3500: Higher-density DIN-rail option for additional PoE, 10G uplinks, or modular expansion.

Implementation notes:

- Use for space-constrained distribution sites, feeder automation, or small remote substations.
- Choose IE3500 over IE3100 when uplink speed, PoE budget, or expansion capacity is the deciding factor.

### Product Configuration Entry Points

The implementation guide contains configuration entry points for:

- IR8340
- IR1101
- IE9300
- IE3100
- IE3500

The product configuration examples are illustrative and should be aligned with final IP addressing, site IDs, SD-WAN templates, utility crypto standards, and operational policy.

#### IR8340 Configuration Steps

Purpose:

- Illustrative IOS XE sequence for substation edge routing, SD-WAN, cellular backup, zone firewall, and IPsec.
- Align system IP, site ID, and interfaces with SD-WAN Manager / controller templates.

Configuration sequence:

1. **SD-WAN fabric membership:** Onboard in SD-WAN Manager, assign system IP, site ID, and OMP, and bind transport interfaces as tunnel sources.
2. **WAN transport interfaces:** Configure primary fiber/MPLS WAN addressing and permit IKE UDP 500/4500 and ESP on WAN ACLs.
3. **Cellular backup interface:** Enable LTE as secondary transport and track reachability so OMP fails over without flapping.
4. **Zone-based firewall:** Define SCADA, MGMT, MULTISERVICE, and WAN zones, with default deny between OT zones and WAN.
5. **SCADA application ACLs:** Permit only DNP3/MMS ports from control-center masters toward IED subnets and log denies.
6. **LAN interfaces toward IE9300:** Use routed links per zone for SCADA, management, and multiservice paths to aggregation switches.
7. **IKEv2 and IPsec profile:** Use AES-GCM transforms, match hub identity, and set lifetimes per utility crypto standards.
8. **Dual-router HA for large sites:** Configure HSRP on SCADA SVI and validate SD-WAN HA so addresses do not duplicate on WAN.

#### IE9300 Configuration Steps

Purpose:

- Layer 2 aggregation for large and medium substations.
- Covers VLANs, uplink trunks, OT resiliency, PoE, and management hardening.

Configuration sequence:

1. **VLAN and SVI plan:** Create SCADA, management, and MultiService VLANs and map them to the utility IP plan and service VPNs on IR8340.
2. **IED access ports:** Use access mode to SCADA VLAN, portfast edge, and storm control on IED-facing ports.
3. **Trunk uplink to IR8340:** Allow only required VLANs on uplinks and trust DSCP where IEDs mark traffic.
4. **REP / MRP redundancy:** Enable ring protocol per design and verify failover timers with the OT vendor.
5. **PoE for cameras and Wi-Fi:** Budget PoE per 1080p camera class and use LLDP for visibility.
6. **Management VLAN security:** Restrict SSH to jump hosts and NMS, and disable insecure management services.
7. **Logging, NTP, and flow export:** Send syslog and Flexible NetFlow to head-end collectors and sync time for correlation.

#### IE3100 Configuration Steps

Purpose:

- Rugged access switching for small sites.
- Covers VLANs, uplink to IR1101, ring resiliency, and management isolation.

Configuration sequence:

1. **Access port VLAN assignment:** Map downlink ports to SCADA VLAN and reserve uplink/combo ports for IR1101 trunk.
2. **RTU / IED access:** Document port-to-bay mapping and enable port security or MAC limits where required.
3. **Uplink trunk to IR1101:** Trunk SCADA and management VLANs on uplink and use port-channel if dual-homed.
4. **REP or MRP ring:** Configure ring resiliency and keep PMU/SCADA on dedicated VLANs where present.
5. **Management VLAN:** Use an L3 SVI on the switch or route through IR1101 only, while blocking direct WAN-to-switch management.
6. **Archive, NTP, and verification:** Archive config before image changes and verify DIN-rail mounting, grounding, and dual DC feed.

#### IE3500 Configuration Steps

Purpose:

- Modular DIN-rail switching for downlink PoE, 10G uplinks, ring protocols, and optional TSN for time-sensitive traffic.

Configuration sequence:

1. **Base port and expansion modules:** Install the correct copper or fiber expansion module and document final port mapping.
2. **VLANs for SCADA, video, and PMU:** Segregate OT, physical security, and enterprise traffic and align with IR8340/IR1101 VRFs.
3. **10G uplink and QoS:** Configure SFP+ uplinks toward the edge router and shape egress if WAN CIR is limited.
4. **PoE budget and camera ports:** Allocate PoE budget and cap per-port draw for 1080p cameras.
5. **MRP / REP / DLR:** Enable redundancy matching the automation vendor, such as DLR for Rockwell or MRP for PROFINET.
6. **TSN frame preemption:** Enable 802.1Qbu / 802.3br only after lab validation with PLC and PMU vendors.
7. **Management and Cyber Vision:** Harden SSH/SNMP and onboard sensor license to Cyber Vision Center.

#### IR1101 Configuration Steps

Purpose:

- Single-box SD-WAN edge for small substations.
- Covers WAN/cellular, zone firewall, LAN SVIs, and overlay verification.

Configuration sequence:

1. **WAN and SD-WAN tunnel bind:** Configure DIA or provider handoff and attach the interface to the SD-WAN tunnel interface.
2. **Cellular transport:** Use built-in or USB LTE as backup transport and verify controller reachability over cellular APN.
3. **SCADA and management LAN SVIs:** Use routed SVIs toward IE3100/IE3500 and keep zones separate for ZBFW policies.
4. **NAT and zone firewall:** Overload SCADA subnet on WAN if required and inspect policies between SCADA and WAN zones.
5. **IPsec / IKEv2:** When not using SD-WAN IPsec only, configure IKEv2 profile and protected tunnel to the hub.
6. **QoS for SCADA vs bulk:** Prioritize DNP3/TCP and PMU UDP on WAN egress and police video if sharing uplink with SCADA.
7. **Verify throughput and logging:** Confirm CPU headroom under scan load and export sampled NetFlow and syslog to NMS.

## Content Maintenance Notes

Use this Markdown file as the portable narrative version of the CVD. The HTML file remains the interactive implementation with diagrams, controls, overlays, calculators, and modal configuration dialogs.

When updating the project:

- Keep the Markdown section order aligned to the left navigation.
- Update this file when adding or renaming menu items.
- Keep traffic profile numbers synchronized with the HTML calculator and profile cards.
- Keep implementation configuration summaries synchronized with the product configuration dialogs.
- Use `design.md` as the reusable structure template for creating other CVDs with the same look and feel.
