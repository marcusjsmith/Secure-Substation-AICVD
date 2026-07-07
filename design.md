# CVD Design Template

Use this document as a reusable structure for building another interactive Cisco Validated Design with the same information architecture, visual tone, and user experience as the Secure Substation AICVD.

## Purpose

Create a single-page, interactive CVD guide that helps a reader move from high-level intent to architecture, design decisions, and implementation patterns without leaving the experience.

The finished guide should feel like an engineering reference, not a marketing site:

- quiet, dark, operational interface
- fixed left navigation
- concise but complete content blocks
- diagrams and flow overlays as the primary visual surface
- clear separation between planning, design rationale, and implementation steps
- reusable card, panel, and modal patterns

## Recommended File Structure

```text
project-root/
  README.md
  design.md
  <cvd-name>.html
  <base-architecture-reference>.html
```

Keep the main experience self-contained in one HTML file unless the CVD grows into a larger application. This makes it easy to publish on GitHub Pages and easy for reviewers to open locally.

## Primary Navigation Model

Use four top-level sections in the left navigation.

```text
Overview
Architecture
Design Guide
Implementation Guide
```

The left menu should stay fixed while the main page scrolls. Submenus appear only when their parent section is active.

## Page Structure

### 1. Overview

Goal: explain what the CVD is, who it is for, and how the rest of the guide is organized.

Recommended content blocks:

- CVD title and one-paragraph value statement
- What the CVD establishes
- Design outcomes
- Guide section map
- Deployment profiles or solution tiers
- Operational and security alignment summary

Template:

```text
<CVD name> overview
  Purpose and scope
  What the design standardizes
  Target environments or personas
  Key outcomes
  Guide sections
```

### 2. Architecture

Goal: show the reference architecture and make important flows selectable.

Use two submenu pages:

```text
Diagram & use cases
Traffic profiles
```

#### Diagram & Use Cases

Use the diagram as the central visual artifact. The left menu should contain selectable use cases such as:

```text
Diagram only
<Primary traffic> Traffic
<Management traffic> Traffic
<Multi-service traffic> Traffic
Controller overlay
Encrypted tunnel / fabric path
Telemetry / observability use case
```

Each selected use case should:

- highlight relevant paths or zones on the diagram
- show a short explanatory panel
- preserve the underlying topology context
- avoid overly long labels in the menu

#### Traffic Profiles

Use this page for planning information rather than architecture diagrams.

Recommended content blocks:

- traffic type definitions
- bandwidth ranges
- deployment tier comparison
- calculator or sizing helper if useful
- planning assumptions and footnotes

Template:

```text
Traffic profiles
  Tier summary
  Traffic classes
  Bandwidth table
  Optional calculator
  Planning notes
```

### 3. Design Guide

Goal: explain design decisions and security controls before showing configuration.

Use submenu pages. A good starting set is:

```text
SD-WAN Design
Zoning / compliance model
Application and inspection policy
```

#### SD-WAN Design

Use this page for controller roles, overlay behavior, segmentation, and security policy capabilities.

Recommended content blocks:

- architecture model
- controller roles
- service VPN or VRF design
- segmentation principles
- security policy capabilities
- reference links

Controller card pattern:

```text
Management plane
  Manager or orchestration component
  Templates, monitoring, lifecycle, policy workflow

Identity plane
  Validator or onboarding component
  Certificate identity, device authorization, discovery

Control plane
  Controller component
  Route exchange, policy distribution, overlay reachability
```

Security capability card pattern:

```text
Segmentation
  Service separation, zones, VRFs, least privilege

Firewall / NGFW
  Permit, deny, inspect, log, application-aware rules

IDS / IPS
  Detection and prevention, signature policy, custom rules

Application recognition
  Application classification, QoS alignment, policy matching
```

#### Zoning / Compliance Model

Use this page to map compliance or operational zones to the architecture.

Recommended content blocks:

- zone definitions
- impact categories
- highlighted architecture zones
- explanatory floating cards
- design implications for policy and implementation

#### Application and Inspection Policy

Use this page to show how traffic classification, firewall rules, and inspection work together.

Recommended content blocks:

- packet-flow diagram
- pass, deny, and inspect paths
- classifier role
- firewall role
- IDS/IPS role
- deployment notes and testing guidance

### 4. Implementation Guide

Goal: turn the design into deployable product and configuration guidance.

Use flow-driven topology boxes, grouped by deployment tier or site profile. Each product tile should open the relevant configuration content.

Recommended profile groups:

```text
Large site
Medium site
Small site
```

Each profile card should include:

- site profile description
- product relationship or basic topology
- product tiles
- short design notes
- links or labels that open configuration dialogs

Configuration content should live in modal dialogs or expandable panels so the main page remains readable.

## Visual System

Use a restrained dark interface.

Recommended tokens:

```text
Background: near-black
Surface: dark panel
Border: muted gray
Primary text: off-white
Secondary text: muted gray
Accent colors: use a small set with domain meaning
Border radius: 6-8px for cards and controls
```

Keep color semantic:

```text
Critical OT / primary flow: warm accent
Management / control: cyan or blue
Fabric / overlay: green or blue
Security / deny / inspect: orange or red
Small or edge profile: yellow or gold
Telemetry / observability: distinct deep blue
```

Typography should use a clear system sans-serif or Inter-style font. Keep the relationship between sizes consistent:

- page titles are largest
- section headings are smaller but clearly dominant
- card headings are compact
- body copy is readable
- captions, badges, and metadata are smallest but not cramped

## Layout Rules

- Use a fixed left navigation panel.
- Keep main content scrollable.
- Avoid landing-page hero layouts.
- Avoid nested cards.
- Use full-width sections or simple grid layouts.
- Use cards only for repeated items, capability blocks, profile tiers, and modals.
- Use concise paragraphs and scannable bullets.
- Keep headings short and literal.
- Use diagrams and flow overlays as the main visual artifact.

## Interaction Patterns

The current CVD uses CSS radio inputs and labels for navigation and state.

Reusable pattern:

```text
Hidden radio inputs
  view-overview
  view-architecture
  view-design-guide
  view-implementation-guide

Section radios
  architecture-page-*
  design-page-*
  flow-*
```

Benefits:

- no application framework required
- works on GitHub Pages
- easy to review as a static file
- labels act as accessible controls when wired correctly

Use checkboxes for:

- opening configuration modals
- dismissing informational panels
- toggling animation

## Content Tone

Use engineering reference language:

- specific
- operational
- design-oriented
- concise
- tied to real workflows

Avoid:

- marketing claims
- generic feature lists without design relevance
- decorative sections that do not help implementation
- long paragraphs inside compact cards

## Adaptation Checklist

When creating a new CVD from this structure:

1. Rename the CVD and update the README.
2. Replace the architecture diagram and flow overlays.
3. Define the deployment tiers or site profiles.
4. Define traffic/use-case menu labels.
5. Build the traffic profile planning page.
6. Add design guide submenu pages for the domain.
7. Add implementation profile cards.
8. Connect product tiles to configuration dialogs.
9. Verify all labels point to real inputs.
10. Check desktop and mobile layouts for text overflow.
11. Publish with GitHub Pages.

## Quality Checklist

Before publishing:

- left navigation remains fixed while content scrolls
- every top-level menu item reveals only its own section
- submenus appear only for active sections
- every label `for` target exists
- no duplicate IDs
- no horizontal text overflow on desktop or mobile
- modals open and close correctly
- diagrams remain readable after font changes
- implementation examples are clearly marked as illustrative where needed
- README links to the GitHub Pages URL

## Reusable Section Outline

```text
Overview
  CVD overview
  What the CVD establishes
  Design outcomes
  Guide sections

Architecture
  Diagram & use cases
    Diagram only
    <traffic/use case 1>
    <traffic/use case 2>
    <controller/fabric use case>
    <telemetry use case>
  Traffic profiles
    Traffic classes
    Bandwidth / scale assumptions
    Tier planning
    Planning notes

Design Guide
  <core architecture design page>
  <zoning/compliance design page>
  <security policy design page>

Implementation Guide
  Large profile
    Product relationship
    Product configuration links
  Medium profile
    Product relationship
    Product configuration links
  Small profile
    Product relationship
    Product configuration links
  Configuration dialogs
```

## Publishing Model

Use GitHub Pages from the `main` branch root. Link directly to the HTML file in `README.md`.

Example:

```markdown
[CVD name](https://<github-user>.github.io/<repo>/<cvd-file>.html)
```
