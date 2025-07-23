# Margo Technical Specification v1.0

## Scope

This specification defines the technical requirements for edge interoperability in industrial automation ecosystems. It specifies the mandatory interfaces, protocols, and behaviors required for Margo-compliant devices, workload orchestration solutions, and application packages.

## Normative References

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

## Terms and Definitions

- **Workload Fleet Management (WFM)**: System responsible for orchestrating and managing containerized workloads across edge devices
- **Edge Device**: Compute hardware that hosts Margo-compliant workloads within customer environments
- **Application Package**: Complete definition of a deployable application including metadata and deployment profiles
- **Management Interface**: API interface between edge devices and workload fleet management solutions
- **Application Registry**: Git repository used to share application packages between application developers and workload orchestration vendors
- **Local Registry**: Container or Helm registry deployed on or near edge devices for storing OCI artifacts
- **Desired State**: Git repository-stored documents defining the intended configuration of applications on a device
- **ApplicationDescription**: YAML document defining application metadata, deployment profiles, and configuration parameters
- **ApplicationDeployment**: Specific instance of an application with user-defined configuration parameters

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

### Application Package Distribution

- Applications SHALL aggregate one or more OCI Containers
- Application packages SHALL be made available in application registries
- Referenced OCI artifacts SHALL be stored in remote or local registries
- Application catalogs and marketplaces are out of scope for Margo

### Configuration Parameters

Application descriptions MAY define configurable parameters. When defined:
- Parameter validation rules MUST be specified using schema definitions
- Parameter values MUST be validated against all rules defined in the schema
- Configuration sections MUST specify how parameters are displayed to users
- The configuration section MUST describe how workload orchestration software vendors display parameters to users
- The schema section MUST describe how workload orchestration software vendors validate user-provided values

### Application Description Schema Elements

#### Metadata Attributes
- `apiVersion`: MUST be set to "margo.org/v1-alpha1"
- `kind`: MUST be set to "ApplicationDescription"
- `metadata.id`: Unique identifier for the application
- `metadata.name`: Human-readable application name
- `metadata.description`: Application description text
- `metadata.version`: Application version string

#### Catalog Attributes
- `catalog.application.icon`: Path to application icon file
- `catalog.application.tagline`: Brief application description
- `catalog.application.descriptionFile`: Path to detailed description file
- `catalog.application.releaseNotes`: Path to release notes file
- `catalog.application.licenseFile`: Path to license file
- `catalog.application.site`: Application website URL
- `catalog.application.tags`: Array of descriptive tags

#### Author and Organization Information
- `author.name`: Author name
- `author.email`: Author email address
- `organization.name`: Organization name  
- `organization.site`: Organization website URL

#### Deployment Profile Requirements
Each deployment profile MUST include:
- `type`: Either "helm.v3" or "compose"
- `components`: Array of deployment components

For Helm deployment profiles:
- `components.name`: Component identifier
- `components.properties.repository`: OCI repository URL
- `components.properties.revision`: Chart version
- `components.properties.wait`: Boolean indicating whether to wait for deployment completion
- `components.properties.timeout`: Optional timeout duration (e.g., "8m30s")

For Compose deployment profiles:
- `components.name`: Component identifier
- `components.properties.packageLocation`: URL to tarball containing compose.yml and artifacts
- `components.properties.keyLocation`: URL to PGP public key for signature verification

#### Parameter Configuration
- `parameters`: Defines configurable parameters with default values
- `parameters.targets`: Specifies where parameter applies in deployment using pointer notation
- `parameters.targets.components`: Array of component names where parameter applies

#### Configuration Layout
- `configuration.sections`: Named sections for organizing parameters
- `configuration.sections.settings`: Individual configuration settings within sections
- `configuration.sections.settings.parameter`: Reference to parameter name
- `configuration.sections.settings.name`: Display name for parameter
- `configuration.sections.settings.description`: Parameter description text

#### Schema Validation
- `schema`: Base class for parameter validation rules
- `schema.dataType`: Parameter data type (string, boolean, integer, double, select)
- Schema subclasses for specific data types with their own validation rules
- Values MUST be validated against all rules defined in the schema

---

## Edge Device Requirements

### Device Compliance

All Margo-compliant edge devices MUST:
- Host a Margo management interface client
- Support containerized workload deployment
- Report device capabilities via the Device API during onboarding
- Monitor assigned Git repositories for desired state changes
- Be onboarded and managed by a single Workload Fleet Manager

### Device Roles and Architecture

Margo devices consist of three major layers:
1. **Margo Interface Layer**: Hosts the Margo management interface client
2. **Platform Layer**: Container runtime and orchestration platform
3. **Traditional Device Layer**: Hardware and base operating system

Device vendors have freedom of implementation regarding specific components (container runtime, orchestration platform) as long as components provide the agreed-upon functionality that application vendors expect.

### Device Capabilities Reporting

- Device owners MUST report device capabilities and characteristics via the Device API when onboarding
- During device lifecycle, capability changes that impact reported characteristics SHALL trigger updated reports to the Workload Fleet Manager
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
  - Device onboarding functionality with the workload orchestration solution
  - Device capabilities reporting
  - Desired state change identification
  - Deployment status reporting

### Authentication and Authorization

- For requests requiring authentication, a bearer token MUST be present in the message's Authorization header
- Access tokens SHALL be obtained by sending requests to the workload orchestration web service's token URL with device client ID and secret
- Authentication SHALL use OAuth2 client credentials grant type
- Authorization header value MUST be set to "Bearer <ACCESS_TOKEN>" for authenticated requests

### State Management

- Workload Fleet Management solutions MUST maintain storage repositories for managed device desired state files
- Device management clients MUST retrieve desired state files from the Workload Fleet Manager
- Device management clients MUST monitor assigned Git repositories for desired state updates using provided URL and access tokens
- Margo does not dictate how desired state files are stored, other than ensuring they are available upon request

### GitOps Requirements

#### Repository Management
- Workload orchestration solutions MUST store device desired state documents within Git repositories that device management clients can access
- Git repository storage provides secure storage and traceability for workload desired states
- Device management clients MUST monitor device Git repositories for updates to desired state
- When differences between current and desired state are detected, devices MUST pull and attempt to apply new desired state

#### State Application Process
- Workload orchestration solutions create desired state documents based on end user inputs for installing, updating, or deleting applications
- Workload orchestration solutions push updates to device Git repositories reflecting desired state changes
- Device management clients monitor assigned Git repositories for changes
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

### Deployment Preferences

- Running device management clients as containerized services is preferred to enable easier lifecycle management but not required

---

## Deployment State Reporting

### State Definitions

Deployment states SHALL be reported as follows:
- **Pending**: Device management client received updated desired state but has not started applying it; when reporting this state, indicate the reason
- **Installing**: Device management client has started applying the desired state
- **Failure**: Desired state application failed; error message and error code MUST be reported
- **Success**: Desired state has been applied completely

### Specific State Conditions

Additional state reporting conditions:
- **Waiting**: Waiting on other applications in the 'Order of operations' to be completed
- Progress reports MUST be sent during state transitions

### Status Reporting Requirements

- Deployment status MUST be sent to the workload orchestration web service using the Device API when deployment state changes
- Status reports inform the workload orchestration web service of current state as new desired state is applied
- When reporting Failure states, error messages and error codes MUST be included
- Status reports MUST include sufficient detail for workload orchestration solutions to track deployment progress

---

## Application Registry Requirements

### Registry Implementation

- Application Developers SHALL use Git repositories to share application packages
- Git repositories used for this purpose are considered Application Registries
- Connectivity between Workload Orchestration Software and Application Registries SHALL be read-only

### Application Package Retrieval

- Upon installation request from end users, Workload Orchestration Vendors SHALL retrieve application packages using git pull requests from Application Registries
- Application packages are made available in application registries while referenced OCI artifacts are stored in remote or local registries

### Registry Access Requirements

- Margo-compliant Workload Orchestration Solutions (WOS) SHALL provide mechanisms for end users to manually configure connections between WOS and application registries
- This requirement ensures end users can install any Margo-compliant application, including applications developed by the end user
- Workload Orchestration Vendors MAY provide enhanced user experience options such as pre-configuring application registries to monitor

### Security Requirements

- Connections between Workload Orchestration software and Application developer application registries SHALL be secured using standard secure connectivity best practices
- Container and Helm artifacts referenced in application manifests MUST be retrievable from configured registries

### Application Discovery

- Application descriptions MUST be retrievable from configured application registries
- Workload Fleet Managers SHALL support querying multiple application registries
- Application metadata MUST be parseable for display in workload catalogs

---

## Workload Observability Requirements

### Observability Scope

Margo's workload observability is limited to the following areas:
- Monitoring container platform health and current state
- Monitoring Workload Fleet Management Client state
- Monitoring compliant workloads deployed to the device

### Observability Data Usage

Workload observability data is intended for:
- **Container Platform Monitoring**: Memory, CPU, disk usage, cluster/node/pod/container availability, run state, and configured resource limits
- **Decision Support**: Enabling end users to determine if devices can support additional workloads or have too many deployed
- **Fleet Management Client Monitoring**: Ensuring management clients run correctly, perform as expected, and don't consume excessive resources
- **Workload Debugging**: Assisting with diagnostics for workloads encountering unexpected conditions

### Observability Limitations

Margo's workload observability is NOT intended to monitor and SHALL NOT be used for monitoring:
- Production processes outside the device
- Industrial machinery
- Controllers
- Sensors
- Any systems external to the edge device itself

### OpenTelemetry Implementation

- Workload observability data SHALL be made available using OpenTelemetry
- OpenTelemetry provides a common specification for observability data generation and consumption

### Observability Signals

Observability data SHALL be captured using the following signals:
- **Metrics**: Numerical measurements over time for observing changes or configured limits (e.g., memory consumption, CPU usage, available disk space)
- **Logs**: Text outputs from running systems/workloads providing information about system events (e.g., security events, errors, unexpected conditions)
- **Traces**: Contextual data for following requests through distributed systems (e.g., bottleneck identification, failure point analysis)

---

## Workload Fleet Management Requirements

### User Experience Requirements

The workload deployment process SHALL support the following workflow:
1. End user visits workload catalog of Workload Fleet Manager Frontend
2. Frontend requests all workloads from Workload Fleet Manager
3. Workload Fleet Manager requests application descriptions from known Application Registries OR services requests from cached application descriptions
4. Frontend parses metadata elements of received application descriptions
5. Frontend presents parsed metadata in UI to end user
6. End user selects workload to install
7. Frontend parses configuration element of selected application description
8. Frontend presents parsed configuration to user for parameter specification
9. End user fills out configurable application parameters
10. Frontend creates ApplicationDeployment definition and sends to Workload Fleet Manager for execution as desired state

### Application Description Processing

- Workload Orchestration Vendors MUST read application description files (margo.yaml)
- Systems MUST present user interfaces allowing specification of parameters available according to application descriptions
- End users specify configuration parameters for application packages before installation
- Application packages are ready for installation process after parameter specification

### Caching and Performance

- Workload Fleet Managers MAY maintain caches of application descriptions to improve performance
- Either real-time registry queries or cached data may be used to service workload catalog requests

---

## Compliance Requirements

### Workload Fleet Management Compliance

A Workload Fleet Management solution is Margo-compliant when it:
- Implements the complete Management API server specification
- Supports both Helm and Compose application deployment methods
- Maintains required device state repositories
- Provides required user configuration interfaces
- Supports Git-based desired state management
- Implements proper OAuth2 authentication flows
- Provides mechanisms for manual application registry configuration

### Device Compliance

An edge device is Margo-compliant when it:
- Implements the complete Management API client specification
- Supports required workload deployment methods (Helm and/or Compose)
- Correctly reports device capabilities during onboarding and when changes occur
- Implements required GitOps monitoring and state application
- Supports proper authentication using OAuth2 client credentials
- Reports deployment states accurately with required error information
- Operates correctly during extended communication downtime periods

### Application Package Compliance

An application package is Margo-compliant when it:
- Follows required package structure with proper file/folder organization
- Contains valid ApplicationDescription YAML with correct apiVersion and kind
- Implements required deployment profile formats (Helm v3 and/or Compose)
- Uses specified security mechanisms when applicable (PGP signing for Compose packages)
- Defines proper configuration parameters with validation schemas
- Includes required metadata for catalog presentation
- References valid OCI containers and deployment artifacts

### Application Registry Compliance

An application registry is Margo-compliant when it:
- Uses Git repositories for application package sharing
- Supports read-only connectivity from Workload Orchestration Solutions
- Implements proper security for repository access
- Provides accessible application descriptions for retrieval
- Supports git pull operations for package retrieval
