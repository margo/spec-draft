# ISO/IEC 23053 Margo Edge Interoperability Standard
## Framework for interoperable orchestration of edge applications and devices in industrial automation ecosystems

### Status: Pre-draft Specification (Work in Progress)
### Version: 1.0-alpha1
### Date: 2025-07-09

---

## Foreword

This document specifies the Margo Edge Interoperability Standard, an open framework for enabling interoperability between edge applications, edge devices, and edge orchestration software in industrial automation ecosystems. This standard is being developed under the auspices of the Linux Foundation's Joint Development Foundation.

**WARNING**: This is a pre-draft specification and SHALL NOT be implemented in production systems or referenced as authoritative. The specification is under active development and subject to substantial changes.

---

## Table of Contents

1. [Scope](#1-scope)
2. [Normative References](#2-normative-references)
3. [Terms and Definitions](#3-terms-and-definitions)
4. [Symbols and Abbreviated Terms](#4-symbols-and-abbreviated-terms)
5. [Conformance](#5-conformance)
6. [Overview and General Principles](#6-overview-and-general-principles)
7. [Personas and Use Cases](#7-personas-and-use-cases)
8. [System Architecture](#8-system-architecture)
9. [Device Requirements and Architecture](#9-device-requirements-and-architecture)
10. [Application Package Specifications](#10-application-package-specifications)
11. [Workload Management Interface](#11-workload-management-interface)
12. [Fleet Management Specifications](#12-fleet-management-specifications)
13. [Observability Framework](#13-observability-framework)
14. [API Specifications](#14-api-specifications)
15. [Interoperability Requirements](#15-interoperability-requirements)
16. [Security Considerations](#16-security-considerations)

### Annexes
- [Annex A (normative) - Application Description Schema](#annex-a-normative---application-description-schema)
- [Annex B (normative) - Deployment State Schema](#annex-b-normative---deployment-state-schema)
- [Annex C (informative) - Implementation Examples](#annex-c-informative---implementation-examples)
- [Annex D (informative) - Best Practices](#annex-d-informative---best-practices)
- [Annex E (normative) - Compliance Test Specifications](#annex-e-normative---compliance-test-specifications)

---

## 1 Scope

### 1.1 General

This International Standard specifies mechanisms for interoperability between edge applications, edge devices, and edge orchestration software in industrial automation ecosystems. It defines:

a) Standardized interfaces for edge device management and workload orchestration
b) Application packaging and deployment specifications
c) Fleet management and observability frameworks
d) Compliance requirements and testing procedures

### 1.2 Application Domain

This standard applies to:
- Industrial automation systems utilizing edge computing
- Multi-vendor edge device deployments
- Containerized workload orchestration at the edge
- Fleet management systems for industrial environments

### 1.3 Out of Scope

This standard does not cover:
- Business logic implementation
- Production process monitoring
- Industrial machinery control protocols
- Sensor data acquisition standards

---

## 2 Normative References

The following documents are referred to in the text in such a way that some or all of their content constitutes requirements of this document:

- RFC 2119, Key words for use in RFCs to Indicate Requirement Levels
- RFC 3339, Date and Time on the Internet: Timestamps
- Open Container Initiative (OCI) Distribution Specification v1.0
- OpenTelemetry Specification v1.0
- Kubernetes API v1.24+
- Docker Compose Specification
- Helm v3 Chart Specification
- OpenGitOps v1.0 Principles

---

## 3 Terms and Definitions

For the purposes of this document, the following terms and definitions apply:

### 3.1 edge compute device
compute hardware that hosts Margo compliant workloads at the edge of industrial networks

### 3.2 workload
software deployed to and running on Margo compliant edge compute devices

### 3.3 workload fleet manager
centralized software solution for managing workload lifecycle across multiple edge devices

### 3.4 application package
distribution mechanism containing application description, resources, and deployment artifacts

### 3.5 deployment profile
configuration specification targeting specific deployment types (Helm v3 or Compose)

### 3.6 desired state
declarative specification of the intended configuration for edge devices and workloads

### 3.7 application registry
Git-based repository for sharing application packages

### 3.8 interoperability
capability of diverse systems and organizations to work together seamlessly

### 3.9 observability
collection and analysis of information produced by a system to monitor its internal behavior

### 3.10 plant operator
individual or team responsible for keeping a manufacturing process operational

---

## 4 Symbols and Abbreviated Terms

| **Acronym** | **Full Form/Definition** |
|-------------|--------------------------|
| API | Application Programming Interface |
| CRD | Custom Resource Definition |
| GitOps | Git Operations |
| JDF | Joint Development Foundation |
| OCI | Open Container Initiative |
| OT | Operational Technology |
| OTLP | OpenTelemetry Protocol |
| PGP | Pretty Good Privacy |
| SDK | Software Development Kit |
| UI | User Interface |
| UUID | Universally Unique Identifier |
| WOS | Workload Orchestration Software |
| YAML | YAML Ain't Markup Language |

---

## 5 Conformance

### 5.1 General Requirements

Conformance to this standard requires the implementation of all mandatory requirements indicated by the keywords "SHALL" and "MUST" as defined in RFC 2119.

### 5.2 Conformance Levels

Three conformance levels are defined:

a) **Device Conformance**: Edge compute devices implementing required interfaces
b) **Application Conformance**: Applications packaged according to specifications
c) **Orchestration Conformance**: Fleet management systems implementing required APIs

### 5.3 Compliance Testing

All implementations claiming conformance SHALL pass the Margo Compliance Test Suite as specified in Annex E.

---

## 6 Overview and General Principles

### 6.1 Mission Statement

Margo's mission emphasizes unlocking innovation barriers in industrial automation through edge interoperability. This is achieved through the creation of a reference implementation, an open standard, and a compliance testing toolkit to facilitate the interoperable orchestration of edge applications and devices.

### 6.2 Core Principles

#### 6.2.1 Openness and Flexibility
Users SHALL have the freedom to choose solutions matching their business needs without vendor lock-in.

#### 6.2.2 Simplification
The framework SHALL reduce time to market and simplify continuous improvement and maintenance.

#### 6.2.3 Innovation Enablement
The system SHALL enable agile systems matching modern business speed requirements.

#### 6.2.4 IT-OT Convergence
The standard SHALL bridge modern IT solutions with operational technology requirements.

### 6.3 Industrial Edge Pain Points Addressed

#### 6.3.1 Workforce Scarcity
The framework addresses the challenge where 75% of industrial companies experience talent shortages by reducing complexity of maintaining and operating infrastructure at scale.

#### 6.3.2 Lifecycle Management Complexity
The standard simplifies deployment, scalability, and operation of industrial solutions in environments with massive increases in compute devices and applications.

#### 6.3.3 Multi-Vendor Environment Challenges
The framework eliminates lengthy and costly bespoke integration work through standardized interfaces.

#### 6.3.4 IT vs OT Architecture Friction
The standard bridges centrally managed, cloud-based IT solutions with decentralized, edge-based OT architectures.

### 6.4 Open Approach Philosophy

#### 6.4.1 Leverage Existing Standards
Margo SHALL adopt existing standards and technologies where possible, embracing proven IT standards and enhancing them as needed for industrial use cases.

#### 6.4.2 Upstream Contributions
The project SHALL make upstream contributions to relevant projects to ensure solutions benefit the broader ecosystem.

#### 6.4.3 Industry Focus
Common building blocks SHALL be provided for the OT landscape without reinventing existing solutions.

---

## 7 Personas and Use Cases

### 7.1 Primary Persona: Plant Operator

#### 7.1.1 Role Definition
The plant operator is an individual or team responsible for keeping a manufacturing process operational, with the greatest decision power for Margo adoption.

#### 7.1.2 Primary Objectives
- Deploy, manage, and maintain a fleet of edge apps and associated hosting devices
- Maximize uptime of their fleet
- Scale operations from single plants to global enterprises

#### 7.1.3 User Journey
The plant operator SHALL be able to:
1. Identify applications from different vendors to improve manufacturing throughput
2. Validate applications in proof-of-concept on test lines
3. Add applications to organization's private catalog
4. Deploy and configure application instances to selected edge devices
5. Configure devices to match application requirements
6. Monitor application performance and confirm improvements
7. Observe the health state with existing observability platforms
8. Scale deployment to hundreds of devices across global facilities
9. Manage fleet-wide updates and maintenance

### 7.2 Secondary Personas

#### 7.2.1 Application Developer
**Primary Goal**: Build, describe, and package applications once for deployment to any Margo-compliant edge device.

**Requirements**:
- Applications SHALL support deployment at scale regardless of fleet management platforms
- Applications SHALL work on any compatible edge compute devices
- Developers SHALL maintain application compatibility across different hosting environments

#### 7.2.2 Device Manufacturer
**Primary Goal**: Build devices once, equipped to host a wide variety of applications for many plant operators.

**Requirements**:
- Devices SHALL be compatible with ALL Margo-compliant Fleet Managers
- Manufacturers SHALL have freedom of implementation for specific components
- Devices MUST provide guaranteed functionality that Application Vendors can depend on

#### 7.2.3 Fleet Management Software Vendor
**Primary Goal**: Build a software platform that manages fleets of applications and devices at scale.

**Requirements**:
- Software SHALL work regardless of the chosen application developers and device manufacturers
- Platform SHALL provide centralized management and observability
- System SHALL handle device orchestration, application deployment, and lifecycle management

#### 7.2.4 System Integrator
**Primary Goal**: Deliver integration services based on reusable deployment patterns.

**Requirements**:
- SHALL provide customized applications and devices
- SHALL allow plant operators to maintain solutions with their choice of fleet management software
- SHALL focus on reusable deployment patterns

#### 7.2.5 Machine Builder
**Primary Goal**: Augment and differentiate machine fleets through application and device combinations.

**Requirements**:
- SHALL provide single-effort deployment capability
- SHALL allow plant operators to deploy and maintain applications with chosen fleet management software
- SHALL focus on value-added differentiation

---

## 8 System Architecture

### 8.1 System Overview

The Margo system architecture consists of four main components:

a) **Workloads**: Software deployed to Margo-compliant edge compute devices
b) **Workload Observability**: Diagnostic information collection using OpenTelemetry
c) **Workload Fleet Management**: Centralized software solution for managing workload lifecycle
d) **Edge Compute Devices**: Compute surfaces where workloads are deployed and run

### 8.2 Three-Layer Architecture

Margo device roles SHALL consist of three major layers:

#### 8.2.1 Margo Interface Layer
- SHALL host the Margo management interface client
- SHALL provide standardized interfaces for workload management
- SHALL ensure compliance with Margo requirements

#### 8.2.2 Platform Layer
- SHALL provide a container runtime environment
- SHALL support Kubernetes, Docker, or Podman platforms
- SHALL abstract underlying hardware differences

#### 8.2.3 Traditional Device Layer
- SHALL provide compute hardware that hosts Margo-compliant workloads
- MAY implement vendor-specific features
- SHALL provide foundation for upper layers

### 8.3 Component Interactions

#### 8.3.1 Data Flow Architecture
```
End User → Workload Fleet Manager Frontend → Workload Fleet Manager → 
Application Registry → Device Management Client → Container Platform
```

#### 8.3.2 GitOps Deployment Pattern
The system SHALL use the OpenGitOps approach for managing Edge device desired state:
- Workload orchestration vendors SHALL maintain Git repositories
- Device management clients SHALL monitor assigned Git repositories for state changes
- State changes SHALL be applied automatically when detected

### 8.4 Software Composition Patterns

#### 8.4.1 Application Description Model
The system SHALL use an application description model to:
- Abstract deployment details
- Support multiple deployment types without special logic
- Enable targeting of different device types

#### 8.4.2 Workload Packaging Structure
```
/
├── application description (margo.yaml)
├── resources/
│   ├── icon
│   ├── license
│   └── documentation
└── deployment artifacts
```

---

## 9 Device Requirements and Architecture

### 9.1 Device Roles

The following device roles are supported:
- **Standalone Cluster**: Independent cluster operation
- **Cluster Leader**: Manages cluster operations
- **Cluster Worker**: Participates in the cluster as a worker node
- **Standalone Device**: Individual device operation

Note: Additional device roles will be introduced as the specification matures.

### 9.2 Core Device Requirements

#### 9.2.1 Mandatory Requirements
- Devices MUST host a Margo management interface client
- The device's management client MUST monitor the device's Git repository for updates
- The device's management client MUST report progress on state changes using the Device API
- When reporting a Failure state, the error message and error code MUST be reported
- The workload orchestration solution MUST store the device's desired state documents within a Git repository

#### 9.2.2 Container Platform Support
Margo-compliant devices SHALL support containerized workloads on:
- **Kubernetes**: Applications packaged as Helm charts (version 3)
- **Docker**: Applications packaged as Compose files in tarball format
- **Podman**: Support for containerized deployments

### 9.3 Device Capability Reporting

#### 9.3.1 Capability Requirements
Devices SHALL report:
- Processor architecture (ARM, x86_64)
- Available memory and storage
- GPU presence and capabilities
- Container runtime support
- Network capabilities

#### 9.3.2 Compatibility Determination
The framework SHALL enable workload fleet managers to:
- Determine which edge compute devices are compatible with workloads
- Validate hardware requirements before deployment
- Match workload requirements to device capabilities

### 9.4 Device Benefits

#### 9.4.1 Universal Compatibility
Margo compliant devices SHALL be compatible with ALL Margo compliant Fleet Managers.

#### 9.4.2 Implementation Freedom
Device vendors SHALL have freedom to implement specific components as long as they provide the agreed upon functionality.

---

## 10 Application Package Specifications

### 10.1 Package Definition and Structure

#### 10.1.1 Required Structure
The application package SHALL have the following file/folder structure:
```
/ # REQUIRED top-level directory
└── application description # REQUIRED YAML document with kind 'ApplicationDescription'
└── resources # OPTIONAL folder with application resources
```

#### 10.1.2 Key Requirements
- There SHALL be only one YAML file in the package root of kind ApplicationDescription
- Applications MUST be packaged as Helm charts AND/OR Compose components
- Digital signing using PGP encryption MUST be used when signing Compose packages

### 10.2 Application Description Schema

#### 10.2.1 Top-Level Structure
```yaml
apiVersion: margo.org/v1-alpha1
kind: ApplicationDescription
metadata: {...}
catalog: {...}
deploymentProfiles: [...]
parameters: {...}
configuration: {...}
schema: [...]
```

#### 10.2.2 Metadata Requirements
The metadata section SHALL include:
- `id` (string, required): Application identifier
- `name` (string, required): Display name
- `description` (string, optional): Description
- `version` (string, required): Version identifier

### 10.3 Deployment Profiles

#### 10.3.1 Supported Types
- **helm.v3**: For Kubernetes-based deployments
- **compose**: For Docker/Podman deployments

#### 10.3.2 Profile Components
Each deployment profile SHALL specify:
- Component name
- Repository or package location
- Version/revision information
- Wait and timeout parameters

### 10.4 Parameter Configuration

#### 10.4.1 Parameter Definition
Parameters SHALL include:
- Name and default value
- Target specifications for deployment
- Validation schema reference

#### 10.4.2 Configuration Sections
The configuration section SHALL organize parameters into logical groups for user interface presentation.

### 10.5 Application Registry

#### 10.5.1 Registry Requirements
- Application developers SHALL use Git repositories as application registries
- Registry connections SHALL be read-only for security
- Workload orchestration vendors SHALL retrieve packages via git pull

#### 10.5.2 Publishing Workflow
1. The developer creates an application package
2. Package stored in Git repository
3. WOS connects to the registry
4. End user browses catalog
5. WOS retrieves the package upon selection
6. User configures parameters
7. Application deployed via desired state

---

## 11 Workload Management Interface

### 11.1 Interface Specifications

#### 11.1.1 Workload Orchestration Service Features
The service SHALL provide:
- API server endpoint for deployment status updates
- Device capabilities reception and management
- GitOps-enabled repository for desired state provision
- Deployment specification storage and retrieval

#### 11.1.2 Device Workload Management Interface
The device interface SHALL include:
- REST API client for sending deployment status updates
- Device capability information transmission
- Git client/library for deployment specification retrieval
- Automatic workload orchestrator updates upon reconfiguration

### 11.2 Deployment Process

#### 11.2.1 GitOps Implementation
The system SHALL use OpenGitOps patterns:
- One Git repository per device
- Desired state expressed as Kubernetes CRDs
- Automatic synchronization upon state changes

#### 11.2.2 Deployment States
The following states SHALL be supported:
- **Pending**: Device received desired state but hasn't started
- **Installing**: Device actively applying desired state
- **Success**: Desired state successfully applied
- **Failure**: Desired state application failed

### 11.3 Container Platform Requirements

#### 11.3.1 Kubernetes Requirements
- Minimum version 1.24+ with CRI-compliant runtime
- Support for Custom Resource Definitions
- Network policy enforcement capabilities
- Persistent volume support

#### 11.3.2 Docker/Podman Requirements
- OCI-compliant container runtime
- Compose specification support
- Volume mounting and network configuration
- Resource limit enforcement

### 11.4 Workload Lifecycle Management

#### 11.4.1 Installation Phase
1. Application selection from catalog
2. Parameter configuration and validation
3. ApplicationDeployment creation
4. Desired state generation and Git update
5. Device synchronization and deployment

#### 11.4.2 Update Phase
1. Configuration changes via fleet manager
2. Desired state modification
3. Git repository update
4. Device detection and application
5. Status reporting and verification

#### 11.4.3 Deletion Phase
1. Removal request via fleet manager
2. Desired state cleanup
3. Git repository update
4. Device cleanup and deallocation

---

## 12 Fleet Management Specifications

### 12.1 Fleet Management Capabilities

#### 12.1.1 Core Capabilities
- Multi-vendor device support
- Scalable orchestration for industrial environments
- Containerized workload support
- Deployment flexibility with Helm and Compose
- Integrated observability

#### 12.1.2 Device Onboarding
The onboarding process SHALL include:
- Fleet Manager assignment
- Management client installation
- Git repository access provisioning
- Compliance validation

### 12.2 GitOps-Based State Management

#### 12.2.1 Repository Management
- Each device SHALL have an assigned Git repository
- Access SHALL be controlled via URLs and tokens
- Changes SHALL be traceable through Git history

#### 12.2.2 Desired State Lifecycle
1. State creation based on user inputs
2. Repository updates with new state
3. Device monitoring for changes
4. State application and reporting
5. Status tracking and verification

### 12.3 Multi-Device Orchestration

#### 12.3.1 Device Role Specialization
- Cluster leaders SHALL coordinate worker nodes
- Workload distribution SHALL match device capabilities
- Resource compatibility SHALL be validated

#### 12.3.2 Fleet Coordination
- Centralized orchestration per device set
- Distributed state via individual repositories
- Unified monitoring across fleet
- Centralized application catalogs

### 12.4 Management Client Requirements

#### 12.4.1 Core Requirements
The management client SHALL:
- Monitor assigned Git repository
- Apply desired state when detected
- Report deployment progress
- Meet interface layer requirements

#### 12.4.2 Security Requirements
- Secure Git repository access
- Support for signed packages
- Integration with authenticated registries

---

## 13 Observability Framework

### 13.1 Framework Overview

#### 13.1.1 Purpose
The observability framework enables collection and analysis of information from industrial edge systems to monitor internal behavior.

#### 13.1.2 Technology Foundation
The framework SHALL be based exclusively on OpenTelemetry specification for vendor-neutral observability.

### 13.2 Observability Signals

#### 13.2.1 Metrics
Numerical measurements over time including:
- CPU and memory usage
- Disk space availability
- Network statistics
- Application-specific metrics

#### 13.2.2 Logs
Text outputs providing:
- Security events
- Error conditions
- System state changes
- Application logs

#### 13.2.3 Traces
Contextual data for:
- Request path tracking
- Performance bottleneck identification
- Failure point analysis
- Distributed system debugging

### 13.3 Scope and Limitations

#### 13.3.1 Included Monitoring
The framework SHALL monitor:
- Device container platform health
- Workload Fleet Management Client state
- Compliant workloads deployed to devices

#### 13.3.2 Excluded Monitoring
The framework SHALL NOT monitor:
- Production processes outside the device
- Industrial machinery and equipment
- Controllers and sensors
- External systems beyond edge boundary

### 13.4 Implementation Requirements

#### 13.4.1 OpenTelemetry Integration
- Applications SHALL respect OTEL environment variables
- Collectors SHALL run on each device
- Data SHALL flow through standardized pipelines
- Both automatic and manual instrumentation SHALL be supported

#### 13.4.2 Data Collection Patterns
- Infrastructure metrics collection
- Container platform monitoring
- Application performance tracking
- Resource consumption analysis

---

## 14 API Specifications

### 14.1 API Versioning

#### 14.1.1 Current Version
- Version identifier: v1-alpha1
- Status: Pre-draft specification
- Format: margo.org/v1-alpha1

### 14.2 Application Package API

#### 14.2.1 ApplicationDescription API
**Purpose**: Present application metadata and define deployment configurations

**Schema Elements**:
- apiVersion: API version identifier
- kind: Must be "ApplicationDescription"
- metadata: Application identification
- catalog: Display information
- deploymentProfiles: Deployment specifications
- parameters: Configurable values
- configuration: UI layout
- schema: Validation rules

### 14.3 Device API

#### 14.3.1 Deployment Status API
**Purpose**: Report deployment state changes

**States**:
- Pending: Received but not started
- Installing: In progress
- Success: Completed successfully
- Failure: Failed with error details

### 14.4 Desired State API

#### 14.4.1 ApplicationDeployment Schema
```yaml
apiVersion: application.margo.org/v1alpha1
kind: ApplicationDeployment
metadata:
  annotations:
    applicationId: <app-id>
    id: <deployment-uuid>
  name: <deployment-name>
  namespace: <namespace>
spec:
  deploymentProfile: {...}
  parameters: {...}
```

### 14.5 Data Models

#### 14.5.1 Common Attributes
All API objects SHALL include:
- apiVersion field
- kind field
- metadata section
- spec or equivalent section

#### 14.5.2 Validation Schemas
The following validation types SHALL be supported:
- TextValidationSchema
- BooleanValidationSchema
- NumericIntegerValidationSchema
- NumericDoubleValidationSchema
- SelectValidationSchema

---

## 15 Interoperability Requirements

### 15.1 Cross-Vendor Compatibility

#### 15.1.1 Device Interoperability
- All Margo-compliant devices MUST work with all compliant fleet managers
- Devices SHALL support standardized APIs
- Capability reporting SHALL use common formats

#### 15.1.2 Application Interoperability
- Applications SHALL deploy consistently across compliant devices
- Package formats SHALL be standardized
- Configuration interfaces SHALL be uniform

### 15.2 Registry Interactions

#### 15.2.1 Application Registry Standards
- Git-based repositories for packages
- Read-only access patterns
- Standardized package retrieval

#### 15.2.2 Container Registry Support
- OCI-compliant registries
- Authenticated access support
- Multi-architecture image support

### 15.3 Compliance Testing

#### 15.3.1 Test Framework
- Open compliance test suite
- Publicly traceable results
- Automated testing procedures

#### 15.3.2 Compliance Areas
- Interface compliance
- Interoperability validation
- Security verification
- Performance testing

### 15.4 Standards Alignment

#### 15.4.1 Leveraged Standards
- Open Container Initiative compliance
- CNCF project integration
- OpenTelemetry observability
- OpenGitOps principles

#### 15.4.2 Vendor Neutrality
- No vendor lock-in
- Cross-vendor compatibility
- Standardized packaging
- Interoperable interfaces

---

## 16 Security Considerations

### 16.1 Package Security

#### 16.1.1 Digital Signing
- Compose packages MUST be digitally signed
- PGP encryption SHALL be used
- Public keys SHALL be distributed securely

#### 16.1.2 Registry Security
- Secure container registries SHALL be supported
- Authentication SHALL be implemented
- Access control SHALL be enforced

### 16.2 Communication Security

#### 16.2.1 Network Security
- Standard secure connectivity SHALL be used
- TLS/HTTPS for data transmission
- Certificate validation SHALL be enforced

#### 16.2.2 Authentication
- Token-based authentication for GitOps
- Registry authentication support
- Device authentication mechanisms

### 16.3 Operational Security

#### 16.3.1 Access Control
- Role-based access control
- Least privilege principles
- Audit logging capabilities

#### 16.3.2 Data Protection
- Sensitive data encryption
- Secure credential storage
- Configuration protection

---

## Annex A (normative) - Application Description Schema

### A.1 Complete Schema Definition

```yaml
apiVersion: margo.org/v1-alpha1
kind: ApplicationDescription
metadata:
  id: string # Required, lowercase-numbers-dashes, max 200 chars
  name: string # Required, display name
  description: string # Optional
  version: string # Required
catalog:
  application:
    descriptionFile: string # Link to markdown
    icon: string # PNG format
    licenseFile: string # Text/markdown/PDF
    releaseNotes: string # Markdown/PDF
    site: string # Website URL
    tagline: string # Slogan
    tags: [string] # Categories
  author:
    - name: string
      email: string
  organization:
    - name: string # Required
      site: string
deploymentProfiles:
  - type: string # helm.v3 or compose
    components:
      - name: string # Required, unique
        properties: # Type-specific
parameters:
  parameterName:
    value: any # Default value
    targets:
      - pointer: string # Target path
        components: [string]
configuration:
  sections:
    - name: string
      settings:
        - parameter: string
          name: string
          description: string
          schema: string
          immutable: boolean
schema:
  - name: string
    dataType: string
    # Additional type-specific fields
```

### A.2 Metadata Specifications

All metadata fields SHALL conform to the following requirements:
- ID fields use lowercase letters, numbers, and dashes only
- Version fields follow semantic versioning
- URLs must be valid HTTP/HTTPS addresses
- File paths are relative to package root

### A.3 Deployment Profile Properties

#### A.3.1 Helm v3 Properties
- repository: OCI URL (required)
- revision: Chart version (required)
- wait: Boolean (default true)
- timeout: Duration string

#### A.3.2 Compose Properties
- packageLocation: URL (required)
- keyLocation: PGP key URL
- wait: Boolean (default true)
- timeout: Duration string

---

## Annex B (normative) - Deployment State Schema

### B.1 ApplicationDeployment Schema

```yaml
apiVersion: application.margo.org/v1alpha1
kind: ApplicationDeployment
metadata:
  annotations:
    applicationId: string # Must match ApplicationDescription
    id: string # UUID
  name: string
  namespace: string
spec:
  deploymentProfile:
    type: string # helm.v3 or compose
    components:
      - name: string
        properties: # Component-specific
  parameters:
    parameterName:
      value: any
      targets:
        - pointer: string
          components: [string]
```

### B.2 State Transition Requirements

Deployment states SHALL transition according to:
- Pending → Installing → Success
- Pending → Installing → Failure
- No reverse transitions allowed
- Failure states must include error details

### B.3 Error Reporting

Failure states SHALL include:
- Error message (human-readable)
- Error code (machine-readable)
- Timestamp of failure
- Component identification

---

## Annex C (informative) - Implementation Examples

### C.1 Simple Application Example

```yaml
apiVersion: margo.org/v1-alpha1
kind: ApplicationDescription
metadata:
  id: com-example-hello-world
  name: Hello World
  description: Basic hello world application
  version: "1.0"
catalog:
  application:
    icon: ./resources/icon.png
    tagline: Simple greeting application
deploymentProfiles:
  - type: helm.v3
    components:
      - name: hello-world
        properties:
          repository: oci://registry/charts/hello
          revision: 1.0.0
parameters:
  greeting:
    value: "Hello"
    targets:
      - pointer: config.greeting
        components: ["hello-world"]
configuration:
  sections:
    - name: General
      settings:
        - parameter: greeting
          name: Greeting Text
          schema: text
schema:
  - name: text
    dataType: string
    maxLength: 50
```

### C.2 Complex Multi-Component Application

```yaml
apiVersion: margo.org/v1-alpha1
kind: ApplicationDescription
metadata:
  id: com-example-analytics
  name: Edge Analytics Platform
  version: "2.1.0"
deploymentProfiles:
  - type: helm.v3
    components:
      - name: database
        properties:
          repository: oci://registry/database
          revision: 3.2.1
      - name: analytics
        properties:
          repository: oci://registry/analytics
          revision: 2.1.0
  - type: compose
    components:
      - name: analytics-docker
        properties:
          packageLocation: https://example.com/analytics.tar.gz
          keyLocation: https://example.com/key.asc
# Additional configuration...
```

### C.3 Deployment Configuration Example

```yaml
apiVersion: application.margo.org/v1alpha1
kind: ApplicationDeployment
metadata:
  annotations:
    applicationId: com-example-analytics
    id: 550e8400-e29b-41d4-a716-446655440000
  name: analytics-deployment
  namespace: production
spec:
  deploymentProfile:
    type: helm.v3
    components:
      - name: database
        properties:
          repository: oci://registry/database
          revision: 3.2.1
      - name: analytics
        properties:
          repository: oci://registry/analytics
          revision: 2.1.0
  parameters:
    dbSize:
      value: "100Gi"
      targets:
        - pointer: storage.size
          components: ["database"]
```

---

## Annex D (informative) - Best Practices

### D.1 Application Development Best Practices

#### D.1.1 Dual Deployment Support
Applications SHOULD provide both Helm and Compose deployment profiles for maximum compatibility.

#### D.1.2 Parameter Design
- Group related parameters logically
- Provide sensible defaults
- Include comprehensive validation
- Document all parameters clearly

#### D.1.3 Resource Management
- Define CPU and memory limits
- Specify minimum requirements
- Support resource scaling
- Monitor resource usage

### D.2 Device Implementation Best Practices

#### D.2.1 Git Repository Management
- Implement efficient polling mechanisms
- Handle network interruptions gracefully
- Maintain local state cache
- Support rollback capabilities

#### D.2.2 Container Platform Selection
- Choose platforms based on use case
- Consider resource constraints
- Implement proper isolation
- Support multi-tenancy where needed

### D.3 Fleet Management Best Practices

#### D.3.1 Scalability Considerations
- Design for thousands of devices
- Implement efficient state distribution
- Use caching appropriately
- Monitor system performance

#### D.3.2 Security Implementation
- Encrypt all communications
- Implement strong authentication
- Audit all operations
- Regular security updates

### D.4 Observability Best Practices

#### D.4.1 Data Collection
- Collect essential metrics only
- Implement sampling for high-volume data
- Use structured logging
- Correlate traces with logs

#### D.4.2 Storage and Retention
- Define retention policies
- Implement data aggregation
- Archive historical data
- Ensure data privacy

---

## Annex E (normative) - Compliance Test Specifications

### E.1 Test Framework Overview

#### E.1.1 Test Categories
- Interface compliance tests
- Interoperability validation
- Security verification
- Performance benchmarks

#### E.1.2 Test Execution
All tests SHALL be:
- Automated where possible
- Reproducible
- Publicly documented
- Version controlled

### E.2 Device Compliance Tests

#### E.2.1 Interface Tests
- Git repository monitoring
- State application verification
- API implementation validation
- Error handling verification

#### E.2.2 Platform Tests
- Container runtime validation
- Resource management verification
- Network configuration tests
- Storage functionality tests

### E.3 Application Compliance Tests

#### E.3.1 Package Structure
- YAML schema validation
- File structure verification
- Resource availability checks
- Deployment profile validation

#### E.3.2 Deployment Tests
- Installation verification
- Parameter application tests
- Update functionality
- Removal verification

### E.4 Orchestration Compliance Tests

#### E.4.1 Management Tests
- Device onboarding validation
- State management verification
- Multi-device coordination
- Catalog functionality

#### E.4.2 Integration Tests
- Cross-vendor scenarios
- End-to-end workflows
- Scalability validation
- Performance benchmarks

### E.5 Test Results and Certification

#### E.5.1 Results Format
Test results SHALL include:
- Test execution timestamp
- Version information
- Pass/fail status
- Detailed error logs

#### E.5.2 Certification Process
- Submit test results
- Independent verification
- Public results posting
- Continuous monitoring

---

## Document Information

- **Copyright**: © 2024 Margo, Joint Development Foundation Projects, LLC
- **License**: Royalty-free open standard
- **Status**: Pre-draft specification (not for production use)
- **Repository**: github.com/margo/specification
- **Website**: specification.margo.org
