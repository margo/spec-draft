# Margo Technical Specification vx.y.z

## Scope

This specification defines the technical requirements for edge interoperability in industrial automation ecosystems. It specifies the mandatory interfaces, protocols, and behaviors required for Margo-compliant devices, workload orchestration solutions, and application packages.

## Normative References

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

## Terms and Definitions

- **Workload Fleet Management (WFM)**: System responsible for orchestrating and managing containerized workloads across edge devices
- **Edge Device**: Compute hardware that hosts Margo-compliant workloads within customer environments
- **Application Package**: Complete definition of a deployable application including metadata and deployment profiles
- **Management Interface**: API interface between edge devices and workload fleet management solutions

---

## Application Package Requirements

### Application Package Structure

An application package SHALL comprise:
- One application description YAML document with element `kind` defined as `ApplicationDescription`
- Optional resources folder containing application metadata files

The application package file structure SHALL be:
```
/ # REQUIRED top-level directory
└── application description # REQUIRED YAML document (e.g., 'margo.yaml')
└── resources # OPTIONAL folder with application resources
```

### Application Description Requirements

- There SHALL be only one YAML file in the package root of kind `ApplicationDescription`
- The deployment profiles specified in the application description SHALL be defined as Helm Charts AND/OR Compose components
- Applications targeting Kubernetes devices MUST be packaged as Helm charts using Helm version 3
- Applications targeting Compose-based devices MUST be packaged as tarball files containing the `compose.yml` file and referenced artifacts
- When digitally signing packages, PGP encryption MUST be used

### Configuration Parameters

Application descriptions MAY define configurable parameters. When defined:
- Parameter validation rules MUST be specified using schema definitions
- Parameter values MUST be validated against all rules defined in the schema
- Configuration sections MUST specify how parameters are displayed to users

---

## Edge Device Requirements

### Device Compliance

All Margo-compliant edge devices MUST:
- Host a Margo management interface client
- Support containerized workload deployment
- Report device capabilities via the Device API during onboarding
- Monitor assigned Git repositories for desired state changes

### Device Capabilities Reporting

- Device owners MUST report device capabilities and characteristics via the Device API when onboarding
- During device lifecycle, capability changes that impact reported characteristics SHALL trigger updated reports
- Capability reports MUST include sufficient information for workload-to-device compatibility determination

### Workload Deployment

- Devices SHALL install applications using either Compose files OR Helm Charts (not both)
- Device management clients MUST support both Helm and Compose deployment methods
- Devices MUST apply desired state changes received via GitOps mechanisms

---

## Management Interface Requirements

### API Implementation Requirements

- Workload Fleet Management vendors MUST implement the server side of the Management API specification
- Device vendors MUST implement a client following the Management API specification
- The Management Interface MUST provide:
  - Device onboarding functionality
  - Device capabilities reporting
  - Desired state change identification
  - Deployment status reporting

### State Management

- Workload Fleet Management solutions MUST maintain storage repositories for managed device desired state files
- Device management clients MUST retrieve desired state files from the Workload Fleet Manager
- Device management clients MUST monitor assigned Git repositories for desired state updates using provided URL and access tokens

### GitOps Requirements

- Device management clients MUST monitor device Git repositories for updates to desired state
- When differences between current and desired state are detected, devices MUST pull and attempt to apply new desired state
- During desired state application, device management clients MUST report progress using the Device API

### Security Requirements

- The Management Interface MUST reference industry security protocols for client and server interactions
- Standard port assignments MUST be used for both client and server interactions
- Interface patterns MUST support extended device communication downtime

### Configuration Requirements

The Management Interface MUST allow end user configuration of:
- **Downtime configuration**: Prevents device management client retries during known downtime periods; communication errors MUST be ignored during configurable downtime periods
- **Polling Interval Period**: Configurable time period indicating hours when device management clients check for desired state updates
- **Polling Interval Rate**: Frequency rate for device management client desired state checks

---

## Deployment State Reporting

### State Definitions

Deployment states SHALL be reported as follows:
- **Pending**: Device management client received updated desired state but has not started applying it
- **Installing**: Device management client has started applying the desired state
- **Failure**: Desired state application failed; error message and error code MUST be reported
- **Success**: Desired state successfully applied

### Status Reporting Requirements

- Deployment status MUST be sent to the workload orchestration web service using the Device API when deployment state changes
- When reporting Failure states, error messages and error codes MUST be included
- Status reports MUST include sufficient detail for workload orchestration solutions to track deployment progress

---

## Application Registry Requirements

### Registry Access

- Margo-compliant Workload Orchestration Solutions (WOS) SHALL provide mechanisms for end users to manually configure connections between WOS and application registries
- Application registry connections MUST support secure communication protocols
- Container and Helm artifacts referenced in application manifests MUST be retrievable from configured registries

### Application Discovery

- Application descriptions MUST be retrievable from configured application registries
- Workload Fleet Managers SHALL support querying multiple application registries
- Application metadata MUST be parseable for display in workload catalogs

---

## Compliance Requirements

### Workload Fleet Management Compliance

A Workload Fleet Management solution is Margo-compliant when it:
- Implements the complete Management API server specification
- Supports both Helm and Compose application deployment methods
- Maintains required device state repositories
- Provides required user configuration interfaces

### Device Compliance

An edge device is Margo-compliant when it:
- Implements the complete Management API client specification
- Supports required workload deployment methods
- Correctly reports device capabilities
- Implements required GitOps monitoring and state application

### Application Package Compliance

An application package is Margo-compliant when it:
- Follows required package structure
- Contains valid ApplicationDescription YAML
- Implements required deployment profile formats
- Uses specified security mechanisms when applicable
