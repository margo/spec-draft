# Margo Edge Interoperability Standard

**Version:** 1.0 (Draft)  
**Date:** July 2025  
**Status:** Work in Progress - Do Not Implement

---

## Table of Contents

- [Introduction](#introduction)
- [System Architecture Overview](#system-architecture-overview)
- [Scope](#scope)
- [Normative References](#normative-references)
- [Terms and Definitions](#terms-and-definitions)
- [Personas and Use Cases](#personas-and-use-cases)
- [Software Composition](#software-composition)
- [Application Interoperability](#application-interoperability)
- [Fleet Management](#fleet-management)
- [Device Interoperability](#device-interoperability)
- [Workload Observability](#workload-observability)
- [API Reference](#api-reference)
- [Security Requirements](#security-requirements)
- [Compliance and Conformance](#compliance-and-conformance)
- [Implementation Guidelines](#implementation-guidelines)

---

# Introduction

The Margo Edge Interoperability Standard addresses the escalating complexity of managing heterogeneous edge computing environments in industrial automation. As manufacturing facilities increasingly deploy compute devices and applications from multiple vendors, organizations encounter significant challenges in lifecycle management, workload orchestration, and operational scalability. This specification establishes a comprehensive framework for seamless interoperability between edge devices, applications, and fleet management systems through standardized APIs, security protocols, and deployment mechanisms. By defining common approaches for application packaging, device onboarding, workload observability, and fleet management, Margo enables organizations to deploy, scale, and operate complex multi-vendor edge environments efficiently while reducing integration barriers and accelerating digital transformation initiatives. 

## What is Margo?

The Margo initiative represents an open collaboration between organizations and individuals sharing a unified vision: delivering edge interoperability for industrial automation ecosystems. 

## Mission Statement

Margo's mission centers on eliminating innovation barriers in industrial automation through edge interoperability. This objective is achieved through the creation of a reference implementation, open standard, and compliance testing toolkit that facilitate interoperable orchestration of edge applications and devices. The Margo initiative aims to accelerate digital transformation by simplifying deployment, scalability, and operation of industrial solutions, enabling organizations to innovate and scale efficiently.

## Addressing Industrial Edge Pain Points

Recent industrial manufacturing trends have created lifecycle management challenges due to the exponential increase in compute devices and applications from multiple suppliers deployed across manufacturing facilities. Margo addresses the complexity of maintaining and operating such infrastructure at scale within multi-vendor environments.

## Applying an Open Approach

Margo cannot address innovation barriers without widespread collaboration and interaction with peer communities. The initiative emphasizes open development and community engagement.

## Core Deliverables

Margo's core deliverables support the mission of enabling interoperability at scale:
- **Reference Implementation**
- **Open Standard** 
- **Open Compliance Test Suite**

# System Architecture Overview

## Envisioned System Design

Margo establishes an open [interoperability](#terms-and-definitions) standard and ecosystem for the industrial edge, enabling [edge compute devices](#edge-compute-device), [workloads](#workload), and [fleet management](#fleet-management) software to achieve compatibility and interoperability across manufacturers and software developers adopting this standard.

<img width="912" height="677" alt="image" src="https://github.com/user-attachments/assets/1a002916-4dc8-453b-a23c-af9bb47ad20d" />

The system architecture comprises the following primary components:

### Workloads
Workloads represent containerized applications deployed to Margo-compliant edge compute devices. These applications are packaged using standardized methods enabling deployment across multiple compatible edge devices while maintaining consistency and reliability. Refer to the [Application Interoperability](#application-interoperability) section for detailed information about supported workloads and packaging methods.

### Workload Observability
Distributed systems require comprehensive diagnostics collection from workloads and systems within the environment. Margo employs the OpenTelemetry specification to capture this information, providing comprehensive monitoring capabilities across the edge ecosystem. The [Workload Observability](#workload-observability) section details Margo's implementation of the OpenTelemetry specification.

### Workload Fleet Management
Workload fleet management software provides centralized lifecycle management for workloads on Margo-compliant edge compute devices. This component delivers orchestration, deployment, and management capabilities across distributed edge environments. Refer to the [Fleet Management](#fleet-management) section for comprehensive details.

### Edge Compute Devices
Edge compute devices serve as the computational substrate for workload deployment and execution. The Margo initiative establishes prescriptive requirements for edge compute device configuration to ensure Margo compliance, guaranteeing functionality and interoperability. The [Device Interoperability](#device-interoperability) section provides detailed specifications.

## Software Composition Overview

Applications exist in distinct stages:
- **"Application Packaging":** Application preparation and readiness for deployment
- **"Application Deployment" (Runtime):** Application availability and accessibility on the device

Note: An intermediate "Application Staging" stage exists where applications are configured and prepared for deployment but not yet initiated. This stage remains outside the current Margo specification scope and is detailed in the [Software Composition](#software-composition) section.

## Device Roles

Supported device roles include:
- **Standalone Cluster** (Leader and/or Worker)
- **Cluster Leader**
- **Cluster Worker** 
- **Standalone Device**

Note: Additional device roles will be introduced as the specification evolves. Comprehensive details are provided in the [Device Interoperability](#device-interoperability) section.

## Margo Device Layers

Margo device roles consist of three fundamental layers:
- **Margo Interface Layer**
- **Platform Layer**
- **Traditional Device Layer**

<img width="678" height="430" alt="image" src="https://github.com/user-attachments/assets/6a0690c5-6dfb-43b9-b386-e54946fc057c" />

While Margo mandates compliance with its requirements, including hosting the Margo management interface client, device vendors retain implementation flexibility. Complete specifications are available in the [Device Interoperability](#device-interoperability) section.

# Scope

This specification defines technical requirements and interfaces for achieving edge interoperability in industrial automation environments, encompassing:

- Application packaging and deployment standards
- Device management and orchestration interfaces
- Fleet management protocols and APIs
- Workload observability and monitoring requirements
- Security and authentication mechanisms
- Compliance testing frameworks

# Normative References

This specification references the following documents:
- ISO/IEC 20922:2016 (MQTT)
- OCI Container Specification
- Kubernetes API Specification
- Helm Chart Specification v3
- Docker Compose Specification
- OpenTelemetry Specification
- OpenGitOps Specification

# Terms and Definitions

## Core Concepts

**Interoperability:** For Margo, interoperability encompasses:
- Establishing common approaches for packaging [Components](#component) for deployment as [Workloads](#workload) to any compatible Margo-compliant [Edge Compute Devices](#edge-compute-device) via any Margo-compliant [Workload Fleet Manager](#workload-fleet-manager)
- Defining standardized approaches for packaging device software and firmware updates for deployment to any Margo-compliant [Edge Compute Devices](#edge-compute-device) via any Margo-compliant [Device Fleet Manager](#device-fleet-manager)
- Establishing common APIs enabling communication between any Margo-compliant [Edge Compute Devices](#edge-compute-device) and any Margo-compliant [fleet management](#fleet-management) software
- Defining standardized approaches for collecting and transmitting diagnostics and observability data from Margo-compliant [Edge Compute Devices](#edge-compute-device)

**Orchestration:** For Margo, orchestration encompasses the deployment of [Workloads](#workload) and device software updates to Margo-compliant [Edge Compute Devices](#edge-compute-device) via Margo-compliant [fleet management](#fleet-management) software. Margo leverages existing container orchestration platforms such as Kubernetes, Docker, and Podman on [Edge Compute Devices](#edge-compute-device) without attempting to duplicate their functionality.

**Fleet Management:** Fleet Management represents a methodology enabling users to manage multiple [workloads](#workload) and devices within their infrastructure. Various fleet management strategies exist, including canary deployments, rolling deployments, and others.

**State Seeking:** The state seeking methodology, adopted by Margo, operates through the [Workload Fleet Manager](#workload-fleet-manager) establishing the "Desired state." The [Edge Device](#edge-compute-device) reconciles its "Current state" with the "Desired state" provided by the Fleet Manager and reports status.

**Provider Model:** The provider model within Margo describes services capable of orchestrating or implementing desired states within [Edge Devices](#edge-compute-device). Currently supported providers include: Helm Client, Compose Client.

## Technical Terms

**Application:** An application comprises one or more [Components](#component), as defined by an Application Description, and bundled within an [application package](#application-package).

**Application Package:** An Application Package distributes an [application](#application). According to the specification, it consists of a folder in a Git repository referenced by URL, containing the Application Description (referencing contained and deployable [Components](#component)) and associated resources (e.g., icons).

**Component:** A Component represents software designed for deployment within customer environments on [Edge Compute Devices](#edge-compute-device). Currently supported Margo components include:
- Helm Chart
- Compose Archive

**Workload:** A Workload represents a [Component](#component) instance running within customer environments on [Edge Compute Devices](#edge-compute-device).

**Edge Compute Device:** Edge Compute Devices represent computational hardware operating within customer environments to support Margo-compliant [Workloads](#workload). These devices host Margo-compliant management agents, container orchestration platforms, and device operating systems. Margo Edge Compute Devices are defined by their functional roles within the Margo Architecture.

**Workload Fleet Manager:** Workload Fleet Manager (WFM) represents software enabling end users to configure, deploy, and manage edge [Workloads](#workload) as fleets on registered [Edge Devices](#edge-compute-device).

**Workload Fleet Management Client:** The Workload Fleet Management Client operates as a service on [Edge Compute Devices](#edge-compute-device), communicating with the [Workload Fleet Manager](#workload-fleet-manager) to receive [Components](#component) for instantiation as [Workloads](#workload) and configurations for application on [Edge Compute Devices](#edge-compute-device).

**Device Fleet Manager:** Device Fleet Manager (DFM) represents software enabling end users to onboard, delete, and maintain [Edge Compute Devices](#edge-compute-device) within the ecosystem. This software operates in conjunction with [Workload Fleet Manager](#workload-fleet-manager) software to provide comprehensive [Edge Device](#edge-compute-device) and [Workload](#workload) management capabilities.

**Device Fleet Management Client:** The Device Fleet Management Client operates as a service on [Edge Compute Devices](#edge-compute-device), communicating with the [Device Fleet Manager](#device-fleet-manager) to receive device configurations for application on [Edge Compute Devices](#edge-compute-device).

**Application Registry:** An Application Registry stores [Application](#application) Packages, enabling developers to distribute applications. Application Registries MUST be Git repositories. [Workload Fleet Managers](#workload-fleet-manager) cannot access Application Registries directly; they access [Application Catalogs](#application-catalog) exclusively.

**Application Catalog:** An Application Catalog stores [Application](#application) Packages preselected for installation readiness within edge environments, enabling [Workload Fleet Managers](#workload-fleet-manager) to deploy them to managed [Edge Compute Devices](#edge-compute-device). Application Catalogs obtain applications from one or more [Application Registries](#application-registry).

**Component Registry:** A Component Registry stores [Components](#component) (e.g., Helm Charts and Compose Archives) for Application Packages. During application deployment through [Workload Fleet Managers](#workload-fleet-manager), components (referenced within Application Descriptions) are retrieved from Component Registries. This may be implemented as an OCI Registry.

**Workload Marketplace:** Workload Marketplace represents the location where end users purchase access rights to [Workloads](#workload) from vendors. Functional requirements include: providing users with available [Workloads](#workload) for purchase, enabling purchase of access rights to [Workloads](#workload), and providing users with metadata for accessing associated Workload Registries/Repositories. Note: The Workload Marketplace component remains outside Project Margo's scope.

# Personas and Use Cases

## Persona Categories

<img width="1053" height="354" alt="image" src="https://github.com/user-attachments/assets/9c91a9de-7389-485d-bfbb-e2c229c6c7cc" />

### End Users
- Evaluate optimal products offered by suppliers
- Consume supplier-provided products
- Integrate multiple vendor products into productive automation systems

### Suppliers
- Contribute domain knowledge and expertise to relevant specification sections
- Develop products compliant with Margo specifications
- Market and sell products to end users

## Detailed Persona Definitions

### End User Personas

**OT User**
Consumes functionality provided by application vendors to operate critical and non-critical business functions or improve business efficiency

**OT Architect**
Establishes and enforces standards across OT deployment locations or sites for enhanced supportability and consistency

**Integrator**
External persona, outside the organization of other end user personas, responsible for assembling and installing hardware and software provided by suppliers

**IT Service Provider**
Delivers IT services, including connectivity, backup and restore, automation, security, and auditing, within end user OT environments

### Supplier Personas

**Workload Supplier**
Provides applications that perform desired functions, such as computer vision or software-defined control, deployed to devices via [Workload Fleet Managers](#workload-fleet-manager)

**Fleet Management Supplier**
Provides software packages enabling end users to manage [workloads](#workload) and devices through [Fleet management](#fleet-management) methodologies

**Device Supplier**
Provides hardware resources, including CPU and memory, along with lifecycle support such as firmware and BIOS updates

**Platform Supplier**
Provides operating system-level software to abstract hardware resources and, optionally, container orchestration software above the operating system layer

## Multiple Personas per Entity

![image](https://github.com/user-attachments/assets/59a59b28-9c78-4260-b80f-6ecfecc33216)

Single entities, such as companies or organizations, may fulfill multiple roles within the Margo-enabled ecosystem:

### Combination of Device Supplier and Platform Supplier
A supplier may provide both underlying hardware (device supplier) and platform services (platform provider), offering "ready-to-deploy" solutions for end user consumption.

### Combination of Workload Supplier and OT User
End user personas, such as OT users, may internally develop applications and leverage Margo specifications to streamline deployment to devices within their environments.

# Software Composition

## Terminology Scoping

This section defines terminology specific to software packaging and deployment stages:

**Application:** The term [application](#application) applies to all stages.

**Workload:** The term [Workload](#workload) applies exclusively to running software (deployment stage). [Workloads](#workload) result from deploying [Components](#component).

**Component:** The term [Component](#component) applies to resources available in packaged software deployed as [Workloads](#workload). Some [providers](#provider-model) may support multiple [Workload](#workload) replica deployment from a single [Component](#component). [Components](#component) are distributed through [Component Registries](#component-registry).

Components assume different forms based on type and stage:
- **Helm v3 as [Component](#component):** Helm Chart
- **Helm v3 as [Workload](#workload):** All container images required by pods to be started
- **Compose as [Component](#component):** Compose Archive
- **Compose as [Workload](#workload):** Compose file and all container images required by services to be started

## Software Packaging Stage

![image](https://github.com/user-attachments/assets/03b578bf-180e-4f72-a776-bb0fc4baf443)

Software at rest is distributed as [Application Packages](#application-package), which are folders with Margo-defined structures comprising software applications. These [Application Packages](#application-package) contain:

- An Application Description representing a Margo-specific method for defining one or more [Components](#component) composition. These [Components](#component) are referenced in Application Description documents, deployable as [workloads](#workload), and provided in Margo-supported formats (e.g., Helm Charts or Compose Archives).
- Application resources: icons, licenses, release notes

[Application Packages](#application-package) and [Components](#component) are managed and hosted separately:
- [Application Registries](#application-registry) store Application Descriptions and associated application resources. [Application Registries](#application-registry) are implemented as Git repositories.
- [Component Registries](#component-registry) store [Components](#component)

The following diagram illustrates the relationship between Application Packages and Components listed within Deployment Profiles:

![image](https://github.com/user-attachments/assets/fe7f7876-9f09-4abf-854b-b86cde441a9d)

The following diagram provides additional details on Component Registry for Compose Deployment Profiles:

![image](https://github.com/user-attachments/assets/d4aa7979-f877-4b85-bf23-8960b3e53673)

Applications and contained components are typically configurable with optional default values.

## Software Deployment Stage

When devices receive instructions to run [Applications](#application) (via desired-state specified with ApplicationDeployment objects), their [Workload Fleet Management Clients](#workload-fleet-management-client) interact with [providers](#provider-model). This process initiates all [Workloads](#workload) required for [Applications](#application) and achieves the desired state.

![image](https://github.com/user-attachments/assets/89643a6a-1003-4845-9214-104d8a218b22)

In this stage, [providers](#provider-model) manage individual [Workloads](#workload).

For Helm v3 Deployment Profiles, [Workload Fleet Management Client](#workload-fleet-management-client) implementations utilize the Helm API to initiate individual Helm Charts.

For Compose Deployment Profiles, [Workload Fleet Management Client](#workload-fleet-management-client) implementations utilize the Compose CLI to initiate individual [Workloads](#workload).

The following diagram shows the result of achieving desired state for Applications with Helm v3 Deployment Profiles (result of helm install):

![image](https://github.com/user-attachments/assets/9518af5c-ca2d-4614-90ef-aff9179d27ff)

The following diagram shows the result of achieving desired state for Applications with Compose Deployment Profiles (result of compose up CLI call):

![image](https://github.com/user-attachments/assets/d8578c95-2304-4265-aab5-35271502b634)

# Application Interoperability

## Workloads Overview

A [workload](#workload) represents software deployed to and executed on Margo-compliant [edge compute devices](#edge-compute-device).

To support Margo's [interoperability](#terms-and-definitions) mission, we initially target containerized [workloads](#workload) capable of running on platforms including Kubernetes, Docker, and Podman. The flexibility these platforms provide enables [workload suppliers](#workload-supplier) to define and package [workloads](#workload) using common methods such as Helm or Compose specifications, facilitating deployment to multiple compatible [edge compute devices](#edge-compute-device).

While Margo initially targets Helm or Compose deployments, we plan to support additional deployment types. Our design goal is to enable [workload fleet managers](#workload-fleet-manager) to support current and future deployment types without implementing specialized logic for each type.

## Application Description Model Goals

Margo's application description model enables [workload fleet managers](#workload-fleet-manager) to:
- Display information about deployable [workloads](#workload) (e.g., [workload catalog](#workload-marketplace))
- Determine [edge compute device](#edge-compute-device) compatibility with [workloads](#workload) (e.g., processor types, GPU presence)
- Capture and validate configuration information from [OT users](#personas-and-use-cases) during [workload](#workload) deployment and updates

Margo's application description model enables [workload suppliers](#workload-supplier) to define multiple deployment profiles within single application description files, targeting different [edge compute device](#edge-compute-device) types (e.g., Arm vs. x86, Kubernetes vs. Docker) without maintaining multiple application description files.

## Packaging

To distribute one or more [workloads](#workload), they are packaged in [application packages](#application-package) provided by [workload suppliers](#workload-supplier) targeting Margo-compliant [edge compute devices](#edge-compute-device). [Workload suppliers](#workload-supplier) create application description YAML documents containing application information and deployment references for OCI-containerized [workloads](#workload) comprising the application. [Application packages](#application-package) are distributed via [application registries](#application-registry), while OCI artifacts are stored in remote or [local registries](#local-registries).

**Example workflow**

The following diagram demonstrates how workload fleet managers might utilize application description information:

<img width="844" height="542" alt="image" src="https://github.com/user-attachments/assets/9612134e-4a7f-4c1c-a2af-91e71b64ac94" />

- End users access workload catalogs of Workload Fleet Manager frontends
- Frontends request all workloads from Workload Fleet Managers
- Workload Fleet Managers either request all application descriptions from known Application Registries or service requests from cached application descriptions
- Workload Fleet Managers return retrieved application description documents to frontends
- Frontends parse metadata elements from all received application description documents
- Frontends present parsed metadata in user interfaces
- End users select workloads for installation
- Frontends parse configuration elements from selected application descriptions
- Frontends present parsed configurations to users
- End users complete configurable application parameters for workload application
- Frontends create ApplicationDeployment definitions (from ApplicationDescriptions and completed parameters) and transmit them to Workload Fleet Managers for desired state execution

## Application Package Definition

[Application packages](#application-package) comprise the following elements:

- **Application Description:** A YAML document with `kind` element defined as `ApplicationDescription`, stored in a file (e.g., `margo.yaml`) containing metadata, deployment configurations, and configurable parameters. There SHALL be only one YAML file of kind `ApplicationDescription` in the package root.
- **Resources:** Additional application information (e.g., manuals, icons, release notes, license files) displayable in [application catalogs](#application-catalog) or [marketplaces](#workload-marketplace).

## Package Structure

```
/                           # REQUIRED top-level directory 
└── application description # REQUIRED YAML document with 'kind' element as 'ApplicationDescription' stored in file (e.g., 'margo.yaml')
└── resources               # OPTIONAL folder containing application resources (e.g., icons, license files, release notes) displayable in application catalogs
```

> Note
> Application catalogs or marketplaces remain outside Margo's scope. Exact marketing material requirements shall be defined by application marketplaces beyond outlined mandatory content.

## Deployment Profiles

Deployment profiles specified in application descriptions SHALL be defined as Helm Charts AND/OR Compose components:

- **For Kubernetes devices:** Applications must be packaged as Helm charts using Helm version 3
- **For Compose devices:** Applications must be packaged as tarball files containing compose.yml files and additional artifacts referenced by Compose files. Digital signing of packages is highly recommended. When digitally signing packages, PGP encryption MUST be used.

> Investigation Required: Security review of package definition is planned. During this review, we will revisit Compose tarball file signing methods and discuss secure container registry handling requiring username and password authentication.
> 
> Investigation Required: We need to determine impacts of using third-party Helm charts on Margo compliance.
> 
> Investigation Required: The current specification lacks methods for defining compatibility information (required resources, application dependencies) and required infrastructure services such as storage, message queues/bus, reverse proxies, or authentication/authorization/accounting.

If either profile cannot be implemented, it MAY be omitted, but Margo RECOMMENDS defining deployment profiles as both Helm charts AND Compose components to strengthen interoperability and applicability.

> Note: Devices running applications install applications using either Compose files or Helm Charts, not both.

## Example Workflow

The following workflow demonstrates how [workload fleet managers](#workload-fleet-manager) might utilize application description information:

1. End users visit [workload catalogs](#workload-marketplace) of [Workload Fleet Manager](#workload-fleet-manager) frontends
2. Frontends request all [workloads](#workload) from [Workload Fleet Managers](#workload-fleet-manager)
3. [Workload Fleet Managers](#workload-fleet-manager) either request all application descriptions from known [Application Registries](#application-registry) or maintain cached application descriptions
4. [Workload Fleet Managers](#workload-fleet-manager) return retrieved application description documents to frontends
5. Frontends parse metadata elements from all received application description documents
6. Frontends present parsed metadata in user interfaces
7. End users select [workloads](#workload) for installation
8. Frontends parse configuration elements from selected application descriptions
9. Frontends present parsed configurations to users
10. End users complete configurable application parameters for [workload](#workload) application
11. Frontends create ApplicationDeployment definitions (from ApplicationDescriptions and completed parameters) and transmit them to [Workload Fleet Managers](#workload-fleet-manager) for desired state execution

The Workload Orchestration Vendor reads application description files (`margo.yaml`) and presents user interfaces allowing specification of parameters available according to `margo.yaml`. End users then specify configuration parameters for [application packages](#application-package). The [application packages](#application-package) are then ready for installation processes.

## Application Registry Interaction

Application Developers SHALL use Git repositories to share [application packages](#application-package). These Git repositories are considered [Application Registries](#application-registry).

Connectivity between Workload Orchestration Software and [Application Registries](#application-registry) SHALL be read-only.

Upon installation requests from end users, Workload Orchestration Vendors SHALL retrieve [application packages](#application-package) using git pull requests from [Application Registries](#application-registry).

At minimum, Margo-compliant WOS SHALL provide methods for end users to manually configure connections between WOS and [application registries](#application-registry).

> **Note:** This is required so as not to prohibit end users from being able to install any Margo-compliant application they wish, including applications developed by end users.

The Workload Orchestration Vendor MAY provide enhanced user experience options such as pre-configuring application registries to monitor. These can include application registries from third-party application vendors or their own applications.

> **Note:** The specifics of the installation process are still under discussion: this could be for example a GitOps based approach.

During the installation process, containers referenced in application manifests (Helm Charts or Compose files) are retrieved from container/Helm registries.

### Secure Access to Application Packages

Connections between Workload Orchestration software and Application Developer [application registries](#application-registry) should be secured using standard secure connectivity best practices:
- Basic authentication via HTTPS
- Bearer token authentication
- TLS certificates

## Publishing Workload Observability Data

Compliant [workloads](#workload) MAY expose workload-specific observability data by transmitting data to OpenTelemetry collectors on standalone devices or clusters. While optional, this is highly recommended for distributed diagnostics support.

**Requirements:**
- [Workload suppliers](#workload-supplier) choosing to expose metrics, traces, or logs for OpenTelemetry consumption MUST transmit data to OpenTelemetry collectors using OTLP
- Information required to communicate with device OTEL Collectors is injected into each container using environment variables
- [Workload suppliers](#workload-supplier) SHOULD NOT expect [workloads](#workload) to be auto-instrumented by external elements
- [Workload suppliers](#workload-supplier) MAY choose observability frameworks other than OpenTelemetry, but frameworks MUST be self-contained within [workload](#workload) deployments
- If alternative approaches are taken, [workload suppliers](#workload-supplier) should NOT publish observability data outside devices/clusters using methods other than OpenTelemetry collectors
- If [workload suppliers](#workload-supplier) choose to export data without OpenTelemetry collectors, they MUST NOT do so without end user approval

**How Workload Suppliers Can Publish Observability Data:**
- Use OpenTelemetry SDKs for instrumentation in supported languages
- Configure applications to send telemetry data to device-provided OpenTelemetry collectors
- Ensure compatibility with standard OpenTelemetry protocols (OTLP)
- Follow OpenTelemetry semantic conventions for consistent data structure
- Test observability data collection across different deployment environments

## Local Registries

Local registries provide configuration options for local OCI-based container (or Helm Chart) registry usage. The goal is to avoid reliance on public, Internet-accessible registries for reasons including:
1. Publicly hosted container images or Helm charts could become unavailable
2. Internet connectivity may be unavailable to devices
3. End users may prefer hosting their own registries for security scans and validation

### Device Categories by Connectivity

- **Fully connected device:** Device deployed with Internet access
- **Locally connected device:** Device with local network connectivity and local repository accessibility
- **Air-gapped device:** Device generally disconnected, requiring direct access for configuration

### Local Registry Configuration Options

**Option 1: Container Registry Mirror on Kubernetes Level**

Kubernetes supports registry mirror configuration. For k3s, the file `/etc/rancher/k3s/registries.yaml` can be used:

```yaml
mirrors:
  "docker.io":
    endpoint:
      - "http://<local-registry-ip>:5000"
configs:
  "docker.io":
    auth:
      username: "<username>"
      password: "<password>"
```

**Option 2: Container Registry as Pull-through Cache on Docker Level**

Configure Docker Registry as caching proxy using `config.yml`:

```yaml
version: 0.1
log:
  fields:
    service: registry
storage:
  filesystem:
    rootdirectory: /var/lib/registry
http:
  addr: :5000
  headers:
    X-Content-Type-Options: [nosniff]
health:
  storagedriver:
    enabled: true
    interval: 10s
    threshold: 3
proxy:
  remoteurl: https://registry-1.docker.io
```

**Option 3: Helm Chart Registry as Pull-through Cache**

Setting up ChartMuseum with proxy cache enabled:

```bash
helm repo add chartmuseum https://chartmuseum.github.io/charts
helm repo update
helm install my-chartmuseum chartmuseum/chartmuseum --set env.open.DISABLE_API=false --set env.open.PROXY_CACHE=true
```

# Fleet Management

## Workload Fleet Management Overview

Workload Management represents critical functionality enabling deployment and maintenance of [workloads](#workload) deployed to customer edges to achieve business objectives. To support Margo's [interoperability](#terms-and-definitions) mission, the Margo Management Interface serves as a critical component enabling interoperability between [Workload Fleet Management](#workload-fleet-manager) software vendors and device vendors.

The management interface primary objectives include:
- Enabling [Workload Fleet Managers](#workload-fleet-manager) to onboard and manage [workloads](#workload) on all Margo-compliant devices by hosting the server side of the interface
- Enabling device vendors to build devices including client sides of interfaces, facilitating workload management via all Margo-compliant [fleet managers](#workload-fleet-manager)

## Workload Deployment Sequence

<img width="904" height="568" alt="image" src="https://github.com/user-attachments/assets/fe8670e5-2f5f-418a-9d23-b0cffc0efec6" />

The complete workload deployment sequence includes:

1. End users visit App Catalog pages
2. [Workload Fleet Manager](#workload-fleet-manager) frontends retrieve available [workloads](#workload) lists
3. End users select [workloads](#workload) for installation
4. Frontends retrieve device lists and filter on capability matches
5. End users select devices and complete configurable questions
6. [Workload Fleet Managers](#workload-fleet-manager) create and store new deployment specifications
7. [Workload Fleet Management Clients](#workload-fleet-management-client) continuously monitor for new deployment specifications
8. Clients retrieve deployment specifications and workload manifests
9. Container orchestrators initiate workload installation components
10. Container runtimes retrieve OCI containers
11. [Workload Fleet Management Clients](#workload-fleet-management-client) provide component status updates
12. Complete deployment status is provided and UIs are updated

## Management Interface Requirements

The Management Interface MUST provide the following functionality:
- Device onboarding with workload orchestration solutions
- Device capability reporting
- Desired state change identification
- Deployment status reporting

### Implementation Requirements

- [Workload Fleet Management](#workload-fleet-manager) vendors MUST implement server sides of [API specifications](#api-reference)
- Device vendors MUST implement clients following [API specifications](#api-reference)
- Solutions MUST maintain storage repositories for desired state files (Margo does not dictate desired state file storage methods beyond ensuring availability upon request)
- Device management clients MUST retrieve device desired state files from [Workload Fleet Managers](#workload-fleet-manager)
- Interface patterns MUST support extended device communication downtime
- Management interfaces MUST reference industry [security protocols](#security-requirements) and port assignments for both client and server interactions
- Running device management clients as containerized services is preferred but not required

### Configuration Requirements

The Management Interface MUST enable end-user configuration of:
- **Downtime configuration:** Ensures device management clients do not retry communication during known downtime. Communication errors MUST be ignored during configurable periods.
- **Polling interval period:** Configurable time periods indicating hours during which device management clients check for device desired state updates
- **Polling interval rate:** Frequency at which device management clients check for device desired state updates

## Edge Device Onboarding

The onboarding process includes:

1. End users provide [Workload Fleet Management](#workload-fleet-manager) web service root URLs to device management clients
2. Device management clients download workload orchestration solution vendor public root CA certificates using [Onboarding APIs](#onboarding-api)
3. Context and trust establishment between device management clients and [Workload Fleet Management](#workload-fleet-manager) web services
4. Device management clients use [Onboarding APIs](#onboarding-api) to onboard with workload orchestration services
5. Device management clients receive client IDs, client secrets, and token endpoint URLs for bearer token generation
6. Device management clients receive URLs for Git repositories containing their desired states and associated access tokens for authentication
7. Device capability information is transmitted from devices to workload orchestration web services using [Device APIs](#device-api)

<img width="558" height="942" alt="image" src="https://github.com/user-attachments/assets/f0eae282-7f87-46ff-b101-75ff8bfd5407" />

### Configuring Workload Fleet Management Web Service URLs

To ensure management clients communicate with correct [Workload Fleet Management](#workload-fleet-manager) web services, device management clients require configuration with expected URLs. Device vendors MUST provide methods for end users to manually configure URLs used by device management clients to communicate with user-selected workload orchestration solutions.

### Margo Web API Authentication Method

Margo Web API communication patterns between device management clients and workload orchestration web services must use secure communication channels. Margo requires OAuth 2.0 for authentication.

**API Authorization Strategy:**
- During onboarding, [Workload Fleet Management](#workload-fleet-manager) web services provide management clients with client IDs, client secrets, and token endpoint URLs
- Management clients use this information to create bearer tokens for each request
- Bearer tokens are set in Authorization headers for each web request sent to [Workload Fleet Management](#workload-fleet-manager) web services requiring authorization

### Payload Security Method

Due to limitations using mTLS with common OT infrastructure such as TLS-terminating HTTPS load balancers or HTTPS proxies performing lawful inspection, Margo adopts certificate-based payload signing approaches to protect payloads from tampering.

**Process:**
- During onboarding, end users upload device x.509 certificates to workload orchestration solutions
- Device management clients download root CA certificates using [Onboarding APIs](#onboarding-api)
- Upon completion, both parties can secure their payloads

**Message Envelope Details:**
Once edge devices prepare messages for [Workload Fleet Management](#workload-fleet-manager) web services, they complete the following to secure messages:
1. Device management clients calculate payload digests and signatures
2. Device management clients add envelopes containing:
   - Actual payloads
   - Payload SHAs, signed by device certificates
   - Certificate identifiers (MUST be GUIDs provided by device manufacturers, typically hardware serial numbers)
3. Envelopes are sent as payloads to [Workload Fleet Management](#workload-fleet-manager) web services
4. [Workload Fleet Management](#workload-fleet-manager) web services treat request payloads as envelope structures and receive certificate identifiers
5. [Workload Fleet Management](#workload-fleet-manager) web services compute payload digests and verify signatures using device certificates
6. Payloads are then processed by [Workload Fleet Management](#workload-fleet-manager) web services

### GitOps Service Enrollment
**Authorization methods for Desired State Git Repositories**
- Git access tokens shall be provided to device management clients. These access tokens MUST be tied to dedicated non-user accounts with frequently rotated, short-lived credentials.
- Token rotation support is required

## Device Capability Reporting

Device capability reporting ensures [Workload Fleet Management](#workload-fleet-manager) solutions have information needed to pair [workloads](#workload) with compatible [edge devices](#edge-compute-device).

**Device Capability Reporting Requirements:**
- Device owners MUST report device capabilities and characteristics via [Device APIs](#device-api) when onboarding devices with workload orchestration solutions
- During Edge device lifecycles, if changes impact reported characteristics, device interfaces shall transmit updates to [Workload Fleet Managers](#workload-fleet-manager)

**Required Information:**
- Device ID
- Device Vendor
- Model Number
- Serial Number
- Margo Device Role Designation (Cluster Leader/Worker / Standalone Device)
- Resources available for [workloads](#workload):
  - Memory Capacity
  - Storage Capacity
  - CPU information
  - Device peripherals (i.e., Graphics cards)
  - Network interfaces (WiFi/Ethernet/Cellular)

## Workload Deployment

Margo uses OpenGitOps approaches for managing [edge device](#edge-compute-device) desired states. Workload orchestration solution vendors maintain Git repositories under their control to push desired state updates for each managed device.

> **Action:** The use of GitOps patterns for pulling desired state is still being discussed/investigated.

### Desired State Requirements

> **Note:** Need to investigate best way to construct the Git Repository. Folder structure / Multiple applications per Edge Device/Cluster
> **Note:** This is the recommendation from FluxCD https://fluxcd.io/flux/guides/repository-structure/

- Workload orchestration solutions MUST store device desired state documents within Git repositories accessible to device management clients
- Device management clients MUST monitor device Git repositories for desired state updates using URLs and access tokens provided by workload orchestration solutions during onboarding

> **Note:** Git repository storage was selected to ensure secure storage and traceability pertaining to workload desired states.

### Workload Management Sequence of Operations

**Desired State lifecycle:**
1. Workload orchestration solutions create desired state documents based on end user inputs when installing, updating, or deleting applications
2. Workload orchestration solutions push updates to device Git repositories reflecting desired state changes
3. Device management clients monitor assigned Git repositories for changes
4. When device management clients detect differences between current (running) states and desired states, they MUST retrieve and attempt to apply new desired states

**Applying Desired States:**
- Devices attempt to apply desired states to achieve new current states
- While new desired states are being applied, device management clients MUST report progress on state changes using [Device APIs](#device-api)

### Deployment Status

Deployment status is transmitted to workload orchestration web services using [Device APIs](#device-api) when deployment state changes occur.

**Deployment status rules:**
- State is `Pending` once device management clients receive updated desired states but have not initiated application. When reporting this state, indicate reasons such as:
  - Waiting for policy agents
  - Waiting for other applications in the 'Order of operations' to be completed
- State is `Installing` once device management clients initiate desired state application processes
- State is `Failure` if desired state application fails at any point. When reporting Failure states, error messages and error codes MUST be reported
- State is `Success` once desired states are completely applied

## Consuming Workload Observability Data

[Workload Fleet Management](#workload-fleet-manager) or observability platform suppliers MAY consume [workload observability](#workload-observability) data exported from end user devices to provide valuable end user services.

End users MAY export observability data from Margo-compliant devices to other OpenTelemetry collectors or backends within their environments not located on devices.

# Device Interoperability

## Edge Compute Devices Overview

Within Margo, devices represent computational hardware hosting Margo-compliant [workloads](#workload) within customer environments. Devices are defined by roles they provide to Margo ecosystems. Each role has specific requirements enabling unique functionality such as cluster management. These Margo devices ensure [Workload Developers](#workload-supplier) have guaranteed functionality present in every compliant device. These devices are onboarded and managed by single [Workload Fleet Managers](#workload-fleet-manager).

The Margo specification provides device vendors with the following benefits:
- Margo-compliant devices are compatible with ALL Margo-compliant [Fleet Managers](#workload-fleet-manager)
- Device vendors retain implementation flexibility regarding specific components (e.g., OCI container runtimes) as long as components provide agreed-upon functionality expected by Application Vendors

### Supported device roles include:

- Standalone Cluster
- Cluster Leader
- Cluster Worker
- Standalone Device

## Base Device Requirements

All Margo-compliant devices MUST support:
- **TPM support**
- **Secure Boot**
- **Attestation**
- **Zero Trust Network Access (ZTNA)**

## Device Role Requirements

Within Margo, devices represent computational hardware operating within customer environments to support Margo-compliant [Workloads](#workload). Edge Compute within Margo is initially referenced as a "Device" representing the initial lifecycle stage. Once devices are onboarded within Workload Orchestration Software, they assume roles based on capabilities.

### Cluster Leader Role

The Cluster Leader Role within Margo describes devices capable of managing clusters of one or more connected worker nodes. Additionally, Cluster Leaders can host Margo-compliant [workloads](#workload).

**Functional Requirements:**
- Host [Workload Fleet Management Clients](#workload-fleet-management-client) (enabling functionality outlined in the [Workload Management](#fleet-management) section)
- Enable management functionality via Kubernetes APIs
- Host additional components:
  - Helm Client (Provider)
  - OCI Container Runtime
  - OTEL Collector
  - Policy Agent
- Host [Device Fleet Management Clients](#device-fleet-management-client)

### Cluster Worker Role

The Cluster Worker Role within Margo describes devices with limited computational capacity that can run Margo-compliant [workloads](#workload). This role is managed via Cluster Leaders.

**Functional Requirements:**
- Enable Worker Node functionality within Kubernetes Clusters via kubelet services
- Host additional components:
  - OCI Container Runtime
  - OTEL Collector
  - Policy Agent
- Host [Device Fleet Management Clients](#device-fleet-management-client)

### Standalone Cluster Role

The Standalone Cluster Role within Margo describes devices with additional computational capacity enabling comprehensive ecosystem functionality.

**Functional Requirements:**
- Enable both Cluster Leader and Worker roles concurrently in single devices
- See [Cluster Leader Role](#cluster-leader-role) and [Cluster Worker Role](#cluster-worker-role) for specific requirements
- Standalone Cluster roles can transition to either Cluster Leader or Cluster Worker at later lifecycle deployment stages

### Standalone Device Role

The Standalone Device role represents devices capable of hosting Margo-compliant [Workloads](#workload). This device role is not intended for clustered configurations and typically consists of devices with limited resources for running [workloads](#workload).

**Functional Requirements:**
- Host [Workload Fleet Management Clients](#workload-fleet-management-client) (enabling functionality outlined in the [Workload Management](#fleet-management) section)
- Host additional components:
  - OCI Container Runtime
  - OTEL Collector
  - Policy Agent
- Host [Device Fleet Management Clients](#device-fleet-management-client)

## Collecting Workload Observability Data

Device owners MUST deploy and configure OpenTelemetry collectors on their devices. Device owners MAY choose deployment models they prefer but MUST use one of the following approaches.

### OpenTelemetry Collector Deployment Requirements

- For standalone and clustered devices, there MUST be at least one OpenTelemetry collector deployed to collect required observability data
- Device owners MAY deploy multiple OpenTelemetry collectors with each collector receiving different observability data parts, provided all required observability data is collected.

<img width="627" height="291" alt="image" src="https://github.com/user-attachments/assets/03ab6569-1fe4-4ffd-9a9d-b2182867e600" />

- For multi-node capable clusters, device owners MAY use DaemonSet deployment models to ensure OpenTelemetry collectors run on each node

<img width="668" height="299" alt="image" src="https://github.com/user-attachments/assets/0b10967b-399a-44b8-8652-fa876bfe16c8" />

- For multi-node capable clusters, device owners MUST ensure secure communication between [workloads](#workload) and collectors from one node to collectors on different nodes
- Device owners MUST NOT require sidecar deployment models at this time
- Device owners MUST NOT pre-configure exporters to transmit observability data from devices because end users must control observability data exports
- Device owners MUST NOT attempt to inject auto-instrumentation into any compliant [workloads](#workload) running on devices not owned by device owners

### Container Platform Observability Requirements

**Kubernetes Requirements:**
For devices running Kubernetes, the following represents minimum observability data that MUST be provided:

- **Cluster observability data MUST be collected**
  - Device owners should use Kubernetes Cluster Receivers with default configurations
  - If not using Kubernetes Cluster Receivers, they MUST provide the same output as default configurations
- **Cluster events observability data MUST be collected**
  - Use either Kubernetes Objects Receivers or Kubernetes Events Receivers with default configurations
  - If not using these receivers, they MUST provide the same output as default configurations
- **Node, Pod, and Container observability data MUST be collected**
  - Use Kubelet Stats Receivers with default configurations
  - If not using these receivers, they MUST provide the same output as default configurations
- **Metadata identifying observability data sources MUST be added**
  - Use Kubernetes Attributes Processors with default configurations
  - If not using these processors, they MUST provide the same metadata as default configurations

**Standalone Device Container Platforms:**
For devices running non-clustered container platforms such as Docker or Podman:

- **Container observability data MUST be collected**
  - Use Docker Stats Receivers or Podman Stats Receivers with default configurations
  - If not using these receivers, they MUST provide the same output as default configurations

**General Requirements:**
- Collectors MUST receive data using OLTP formats
- Use OLTP Receivers to enable [workloads](#workload) to transmit observability data to collectors
- **Host observability data MUST be collected**
  - Use Host Metrics Receivers with default configurations
  - If not using these receivers, they MUST provide the same output as default configurations

### Workload Fleet Management Client Observability Requirements

For several reasons, [Workload Fleet Management Clients](#workload-fleet-management-client) should be deployed as containerized [workloads](#workload). If deployed in this manner, [workload](#workload) resource utilization observability data is captured automatically as part of container platform observability requirements.

If device owners choose not to deploy [Workload Fleet Management Clients](#workload-fleet-management-client) as containerized [workloads](#workload), they MUST ensure resource usage observability data is available from OpenTelemetry collectors for their clients.

### Connecting to OpenTelemetry Collectors

To enable [workloads](#workload) to publish observability data to collectors on standalone devices or clusters, device owners MUST inject the following environment variables into each container:

| Environment Variable | Description |
|---------------------|-------------|
| GRPC_OTEL_EXPORTER_OTLP_ENDPOINT | (Optional) URL for [workloads](#workload) to connect to OpenTelemetry collectors using gRPC |
| HTTP_OTEL_EXPORTER_OTLP_ENDPOINT | (Required) URL for [workloads](#workload) to connect to OpenTelemetry collectors using HTTP + protobuf |
| OTEL_EXPORTER_OTLP_CERTIFICATE | (Optional) PATH for client certificates (in PEM format) for secure connections to OpenTelemetry Collectors. Workloads must connect using certificates if provided. |
| OTEL_EXPORTER_OTLP_PROTOCOL | (Optional) "grpc" if preferred protocol is gRPC, "http/protobuf" if preferred protocol is HTTP + protobuf. Default is "http/protobuf" if nothing is provided. If preferred protocol is "grpc" but no gRPC endpoint is provided, or if workload clients cannot connect via gRPC, workload clients connect using "http/protobuf". |

### Exporting Observability Data

End users MUST be able to export observability data from standalone devices or clusters to collectors or backends onsite or in the cloud if they wish to enable remote monitoring and diagnostics.

OpenTelemetry supports both push and pull approaches for retrieving data from collectors. Cloud-based [workload fleet management](#workload-fleet-manager) or observability platform service vendors should NOT require pull methods for collecting observability data because most end users will not expose devices to the internet due to security concerns.

### Workload Observability Default Telemetry

#### Metrics
The following telemetry data is collected using default configurations for receivers indicated above. Additional information about each piece of telemetry is available from receiver documentation.

| Metric Group | Metric | Target | Kubernetes Cluster Receiver | Kubelet Stats Receiver | Docker Stats Receiver | Podman Stats Receiver | Host Metrics Receiver |
|---|---|---|---|---|---|---|---|
| CPU | Limit | Container | | X | | | |
| CPU | Load Average (15m, 5m, 1m) | System | | | | | X |
| CPU | Time | Container, Kubernetes Node, Kubernetes Pod, System | | X | | | X |
| CPU | Request | Container | | X | | | |
| CPU | Usage Kernel Mode | Container | | X | | | |
| CPU | Usage Per CPU | Container | | X | | | |
| CPU | Usage System | Container | | X | | | |
| CPU | Usage Total | Container | | X | | X | |
| CPU | Usage Use Mode | Container | | X | | | |
| CPU | Utilization | Container, Kubernetes Node, Kubernetes Pod | | X | X | | X |
| Disk | IO | Container, System | | X | | | X |
| Disk | IO Read | Container | | X | | | |
| Disk | IO Write | Container | | X | | | |
| Disk | IO Time | System | | | | | X |
| Disk | IO Time (Weighted) | System | | | | | X |
| Disk | Operations | System | | | | | X |
| Disk | Operations Pending | System | | | | | X |
| Disk | Operation Time | System | | | | | X |
| Disk | Total Read/Writes | System | | | | | X |
| File System | Available | Container, Kubernetes Node, Kubernetes Pod | | X | | | |
| File System | Capacity | Container, Kubernetes Node, Kubernetes Pod | | X | | | |
| File System | Inodes | Kubernetes Volume, System | | X | | | X |
| File System | Inodes Free | Volume | | X | | | |
| File System | Inodes Used | Volume | | X | | | |
| File System | Usage | Container, Kubernetes Node, Kubernetes Pod, System | | X | | | X |
| Memory | Available | Container, Kubernetes Node, Kubernetes Pod | | X | | | |
| Memory | File | Container | | X | | | |
| Memory | Limit | Container | | X | X | | X |
| Memory | Major Page Fault | Container, Kubernetes Node, Kubernetes Pod | | X | | | |
| Memory | Page Faults | Container, Kubernetes Node, Kubernetes Pod | | X | | | |
| Memory | Percent | Container | | X | | X | |
| Memory | Request | Container | | X | | | |
| Memory | RSS | Container, Kubernetes Node, Kubernetes Pod | | X | | | |
| Memory | Total Cache | Container | | X | | | |
| Memory | Usage | Container, Kubernetes Node, Kubernetes Pod, System | | X | X | | X |
| Memory | Working Set | Container, Kubernetes Node, Kubernetes Pod | | X | | | |
| Network | Connections | System | | | | | X |
| Network | Errors | Kubernetes Node, Kubernetes Pod, System | | X | | | X |
| Network | IO | Kubernetes Node, Kubernetes Pod, System | | X | | | X |
| Network | IO Bytes Sent | Container | | X | | X | |
| Network | IO Bytes Received | Container | | X | | X | |
| Network | IO Packets | System | | | | | X |
| Network | IO Packets Dropped | System | | | | | X |
| Network | IO Packets Dropped (Incoming) | Container | | X | | | |
| Network | IO Packets Dropped (Outgoing) | Container | | X | | | |
| Paging | Faults | System | | | | | X |
| Paging | Operations | System | | | | | X |
| Paging | Usage | System | | | | | X |
| Process | CPU Time | System | | | | | X |
| Process | Disk IO | System | | | | | X |
| Process | Memory Usage | System | | | | | X |
| Process | Memory Virtual | System | | | | | X |
| Processes | Count | System | | | | | X |
| Processes | Created | System | | | | | X |
| Resource Quota | Hard Limit | Various | X | | | | |
| Resource Quota | Used | Various | X | | | | |
| State | Ready | Container | | X | | | |
| State | Restarts | Container | | X | | | |
| State | Active Jobs | Cron Job | X | | | | |
| State | Current Scheduled Nodes | Daemonset | X | | | | |
| State | Desired Scheduled Nodes | Daemonset | X | | | | |
| State | Misscheduled Modes | Daemonset | X | | | | |
| State | Ready Nodes | Daemonset | X | | | | |
| State | Available | Deployment | X | | | | |
| State | Desired | Deployment | X | | | | |
| State | Current Replicas | HPA | X | | | | |
| State | Desired Replicas | HPA | X | | | | |
| State | Max Replicas | HPA | X | | | | |
| State | Min Replicas | HPA | X | | | | |
| State | Active Pods | Job | X | | | | |
| State | Desired Successful Pods | Job | X | | | | |
| State | Failed Pods | Job | X | | | | |
| State | Max Parallel Jobs | Job | X | | | | |
| State | Successful Pods | Job | X | | | | |
| State | Phase | Namespace | X | | | | |
| State | Phase | Pod | X | | | | |
| State | Available | Replicaset | X | | | | |
| State | Desired | Replicaset | X | | | | |
| State | Available | Replication Controller | X | | | | |
| State | Desired | Replication Controller | X | | | | |
| State | Current Pods | Stateful Set | X | | | | |
| State | Desired Pods | Stateful Set | X | | | | |
| State | Ready Pods | Stateful Set | X | | | | |
| State | Updated Pods | Stateful Set | X | | | | |
| Storage | Available | Volume | X | | | | |
| Storage | Capacity | Volume | X | | | | |
| Storage | Limit | Container | | X | | | |
| Storage (Ephemeral) | Limit | Container | | X | | | |
| Storage | Requests | Container | | X | | | |
| Storage (Ephemeral) | Request | Container | | X | | | |

#### Logs
Kubernetes Events receivers collect event logs using default configurations. Kubernetes Object Receivers must be configured to collect desired logs. Container logs must be emitted using OTLP.

#### Kubernetes Attributes Processor

The following attributes are added to each signal using Kubernetes Attribute Processors' default configurations:

- k8s.namespace.name
- k8s.pod.name
- k8s.pod.uid
- k8s.pod.start_time
- k8s.deployment.name
- k8s.node.name

# Workload Observability

## Overview

Observability encompasses the collection and analysis of information produced by systems to monitor their internal behaviors.

Observability data is captured using the following signals:
- **Metrics:** Numerical measurements in time used to observe changes over periods or configured limits. Examples include memory consumption, CPU usage, and available disk space.
- **Logs:** Text outputs produced by running systems/[workloads](#workload) to provide information about system activities. Examples include outputs capturing security events such as failed login attempts or unexpected conditions such as errors.
- **Traces:** Contextual data used to follow request paths through distributed systems. Trace data can identify bottlenecks or failure points within distributed systems.

## Margo's Workload Observability Scope

Margo's [workload observability](#workload-observability) scope encompasses the following areas:
- Device container platforms
- Device [Workload Fleet Management Clients](#workload-fleet-management-client)
- Compliant [workloads](#workload) deployed to devices

## Intended Use Cases

[Workload observability](#workload-observability) data is intended for purposes including:
- Monitoring container platform health and current states. This includes aspects such as memory, CPU, and disk usage, as well as cluster, node, pod, and container availability, run states, and configured resource limits. This enables end users to make decisions such as whether devices can support additional [workloads](#workload) or have excessive deployments.
- Monitoring [Workload Fleet Management Clients](#workload-fleet-management-client) and containerized [workload](#workload) states to ensure correct operation, expected performance, and appropriate resource consumption.
- Assisting with debugging/diagnostics for [workloads](#workload) encountering unexpected conditions impacting their expected operation.

**Important:** Margo's [workload observability](#workload-observability) is NOT intended for monitoring external elements such as production processes, machinery, controllers, or sensors and should NOT be used for this purpose.

## Workload Observability Framework

[Workload observability](#workload-observability) data is made available using OpenTelemetry. OpenTelemetry, a popular open-source specification, defines common methods for observability data generation and consumption.

**Reasons for selecting OpenTelemetry:**
- OpenTelemetry is based on open-source specifications, not solely implementations
- OpenTelemetry enjoys widespread adoption
- OpenTelemetry has large, active open-source communities
- OpenTelemetry provides SDKs for many popular languages
- The OpenTelemetry community project offers reusable components such as telemetry receivers for Kubernetes, Docker, and host systems, facilitating integration
- OpenTelemetry is vendor-agnostic

# Technical References

## Margo Management API Specification

The Margo Management API enables communication between Margo-compliant devices and orchestration solutions.

## Authorization and Security

### Authorization Header

For requests requiring authentication, bearer tokens MUST be present in message Authorization headers.

Access tokens can be obtained by sending requests to workload orchestration web service token URLs, providing device client IDs and secrets.

```bash
curl -X POST \
-H "Content-Type: application/x-www-form-urlencoded" \
-d "grant_type=client_credentials&client_id=<CLIENT_ID>&client_secret=<CLIENT_SECRET>" \
<WOS_Token_URL>
```

Response:
```json
{
  "access_token": "ACCESS_TOKEN",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

Set Authorization header values to `Bearer <ACCESS_TOKEN>` when making requests requiring authorization.

### Payload Signing

Steps for payload signing:
1. Generate SHA-256 hash values for request bodies
2. Create digital signatures using message source certificate private keys to encrypt hash values
3. Base-64 encode certificate public keys and digital signatures in the format `<public key>;<digital signature>`
4. Include base-64 encoded strings in request `X-Payload-Signature` headers

### Verifying Signed Payloads

Steps for verifying signed payloads:
1. Retrieve public keys from `X-Payload-Signature` headers
2. Decrypt digital signatures using public keys to obtain original hash values
3. Generate SHA-256 hash values for request bodies
4. Ensure generated hash values match hash values from messages

### Desired State API

Desired state is expressed as Kubernetes custom resource definitions and made available to device management clients as YAML documents using OpenGitOps patterns.

**Application Deployment Definition:**

```yaml
apiVersion: application.margo.org/v1alpha1
kind: ApplicationDeployment
metadata:
  annotations:
    id:
    applicationId:
  name:
  namespace:
spec:
  deploymentProfile:
    type:
    components:
      - name:
        properties:
  parameters:
    param:
      value:
      targets:
        - pointer:
          components: []
```

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| apiVersion | string | Y | Identifier of API version followed by object definition |
| kind | string | Y | Must be ApplicationDeployment |
| metadata | Metadata | Y | Metadata element specifying application deployment characteristics |
| spec | Spec | Y | Spec element defining deployment profiles and parameters associated with application deployments |

**Metadata Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| annotations | Annotations | Y | Defines application IDs and unique identifiers associated with deployment specifications |
| name | string | Y | When deploying to Kubernetes, manifest names |
| namespace | string | Y | When deploying to Kubernetes, namespaces where manifests are added |

**Annotations Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| applicationId | string | Y | Application identifiers. Must be lowercase letters and numbers and MAY contain dashes. MUST NOT exceed 200 characters |
| id | string | Y | Unique identifier UUIDs of deployment specifications. Must be assigned by Workload Orchestration Software |

> **Action:** This is incomplete and doesn't contain the details for the deploymentProfile or parameters

> **Action:** We are currently investigating the best way to interface with source control infrastructure

### Complete Application Deployment Examples

#### Example 1: Cluster Enabled Application Deployment Specification

```yaml
apiVersion: application.margo.org/v1alpha1
kind: ApplicationDeployment
metadata:
  annotations:
    applicationId: com-northstartida-digitron-orchestrator
    id: a3e2f5dc-912e-494f-8395-52cf3769bc06
  name: com-northstartida-digitron-orchestrator-deployment
  namespace: margo-poc
spec:
  deploymentProfile:
    type: helm.v3
    components:
      - name: database-services
        properties:
          repository: oci://quay.io/charts/realtime-database-services
          revision: 2.3.7
          timeout: 8m30s
          wait: "true"
      - name: digitron-orchestrator
        properties:
          repository: oci://northstarida.azurecr.io/charts/northstarida-digitron-orchestrator
          revision: 1.0.9
          wait: "true"
  parameters:
    adminName:
      value: Some One
      targets:
        - pointer: administrator.name
          components:
            - digitron-orchestrator
    adminPrincipalName:
      value: someone@somewhere.com
      targets:
        - pointer: administrator.userPrincipalName
          components:
            - digitron-orchestrator
    cpuLimit:
      value: "4"
      targets:
        - pointer: settings.limits.cpu
          components:
            - digitron-orchestrator
    idpClientId:
      value: 123-ABC
      targets:
        - pointer: idp.clientId
          components:
            - digitron-orchestrator
    idpName:
      value: Azure AD
      targets:
        - pointer: idp.name
          components:
            - digitron-orchestrator
    idpProvider:
      value: aad
      targets:
        - pointer: idp.provider
          components:
            - digitron-orchestrator
    idpUrl:
      value: https://123-abc.com
      targets:
        - pointer: idp.providerUrl
          components:
            - digitron-orchestrator
        - pointer: idp.providerMetadata
          components:
            - digitron-orchestrator
    memoryLimit:
      value: "16384"
      targets:
        - pointer: settings.limits.memory
          components:
            - digitron-orchestrator
    pollFrequency:
      value: "120"
      targets:
        - pointer: settings.pollFrequency
          components:
            - digitron-orchestrator
            - database-services
    siteId:
      value: SID-123-ABC
      targets:
        - pointer: settings.siteId
          components:
            - digitron-orchestrator
            - database-services
```

#### Example 2: Standalone Device Application Deployment Specification

```yaml
apiVersion: application.margo.org/v1alpha1
kind: ApplicationDeployment
metadata:
  annotations:
    applicationId: com-northstartida-digitron-orchestrator
    id: ad9b614e-8912-45f4-a523-372358765def
  name: com-northstartida-digitron-orchestrator-deployment
  namespace: margo-poc
spec:
  deploymentProfile:
    type: compose
    components:
      - name: digitron-orchestrator-docker
        properties:
          keyLocation: https://northsitarida.com/digitron/docker/public-key.asc
          packageLocation: https://northsitarida.com/digitron/docker/digitron-orchestrator.tar.gz
  parameters:
    adminName:
      value: Some One
      targets:
        - pointer: ENV.ADMIN_NAME
          components:
            - digitron-orchestrator-docker
    adminPrincipalName:
      value: someone@somewhere.com
      targets:
        - pointer: ENV.ADMIN_PRINCIPALNAME
          components:
            - digitron-orchestrator-docker
    idpClientId:
      value: 123-ABC
      targets:
        - pointer: ENV.IDP_CLIENT_ID
          components:
            - digitron-orchestrator-docker
    idpName:
      value: Azure AD
      targets:
        - pointer: ENV.IDP_NAME
          components:
            - digitron-orchestrator-docker
    idpProvider:
      value: aad
      targets:
        - pointer: ENV.IDP_PROVIDER
          components:
            - digitron-orchestrator-docker
    idpUrl:
      value: https://123-abc.com
      targets:
        - pointer: ENV.IDP_URL
          components:
            - digitron-orchestrator-docker
    pollFrequency:
      value: "120"
      targets:
        - pointer: ENV.POLL_FREQUENCY
          components:
            - digitron-orchestrator-docker
    siteId:
      value: SID-123-ABC
      targets:
        - pointer: ENV.SITE_ID
          components:
            - digitron-orchestrator-docker
```

### Application Package API - Application Description

Application descriptions present applications, e.g., on [application catalogs](#application-catalog) or [marketplaces](#workload-marketplace) from which end users select applications for installation.

**Top-level Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| apiVersion | string | Y | Identifier of API version followed by object definition |
| kind | string | Y | Specifies object type; must be 'application' |
| metadata | Metadata | Y | Metadata element specifying application deployment characteristics |
| deploymentProfiles | []DeploymentProfile | Y | Deployment profiles for applications |
| parameters | map[string]Parameter | N | Configurable parameters for applications |
| configuration | Configuration | N | Configuration layouts and validation rules |

**Metadata Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| id | string | Y | Application identifiers. Must be lowercase letters and numbers and MAY contain dashes. MUST NOT exceed 200 characters |
| name | string | Y | Application official names for display purposes. Can contain whitespace and special characters |
| description | string | N | Application descriptions |
| version | string | Y | Application versions |
| catalog | Catalog | Y | Catalog elements specifying application catalog details |

**Catalog Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| application | ApplicationMetadata | N | Application elements specifying application-specific metadata |
| author | []Author | N | Information about application authors |
| organization | []Organization | N | Information about providing organizations |

**ApplicationMetadata Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| descriptionFile | string | N | Links to files containing application full descriptions. Files should be markdown files |
| icon | string | N | Links to icon files (e.g., PNG format) |
| licenseFile | string | N | Links to files detailing application licenses. Files should be plain text, markdown, or PDF files |
| releaseNotes | string | N | Statements about changes for application releases. Files should be markdown or PDF files |
| site | string | N | Links to application websites |
| tagline | string | N | Application slogans |
| tags | []string | N | Arrays of strings providing additional context for applications in user interfaces to assist with categorizing, searching, etc. |

**Author Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| name | string | N | Names of application creators |
| email | string | N | Email addresses of application creators |

**Organization Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| name | string | Y | Organizations responsible for application development and distribution |
| site | string | N | Links to organization websites |

**DeploymentProfile Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| type | string | Y | Defines deployment configuration types. Allowed values are helm.v3 and compose |
| components | []Component | Y | Component elements indicating components for deployment when installing applications |

**Component Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| name | string | Y | Unique names used to identify component packages. For Helm installations, names are used as chart names. Must be lowercase letters and numbers and MAY contain dashes |
| properties | ComponentProperties | Y | Dictionary elements specifying component package deployment details |

**Parameter Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| name | string | Y | Names of parameters |
| value | various | N | Parameter default values. Accepted data types: string, integer, double, boolean, array[string], array[integer], array[double], array[boolean] |
| targets | []Target | Y | Used to indicate which components values should be applied to when installing or updating applications |

**Target Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| pointer | string | Y | Names of parameters in deployment configurations. For Helm deployments, this is dot notation for matching elements in values.yaml files (same naming convention as --set command line argument with helm install). For Compose deployments, this is environment variable names to set |
| components | []string | Y | Indicates which deployment profile components parameter targets apply to. Component names specified here MUST match component names in deployment profiles |

**Configuration Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| sections | []Section | Y | Sections group related parameters together, enabling user interfaces with logical parameter groupings in each section |

**Section Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| name | string | Y | Section names. May be used in user interfaces to show groupings of associated parameters within sections |
| settings | []Setting | Y | Settings provide instructions to workload orchestration software vendors for displaying parameters to users. Users MUST be able to provide values for all settings |

**Setting Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| parameter | string | Y | Names of parameters referenced in parameter definitions |
| name | string | Y | Display names for parameters in user interfaces |
| description | string | N | Parameter descriptions for user interfaces |
| schema | string | Y | Names of schema rules for parameter validation |
| immutable | bool | N | If true, indicates values cannot be changed after initial installation |

**Schema Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| name | string | Y | Schema rule names referenced in setting definitions |
| dataType | string | Y | Parameter data types (string, integer, double, boolean) |
| allowEmpty | bool | N | If false, indicates values must be provided. Default is false |

**TextValidationSchema Attributes (extends Schema):**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| minLength | integer | N | Minimum number of characters values must have to be valid |
| maxLength | integer | N | Maximum number of characters values must have to be valid |
| regexMatch | string | N | Regular expressions for value validation |

**BooleanValidationSchema Attributes (extends Schema):**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| allowEmpty | bool | N | If false, indicates values must be provided. Default is false |

**NumericIntegerValidationSchema Attributes (extends Schema):**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| minValue | integer | N | Minimum allowed integer values |
| maxValue | integer | N | Maximum allowed integer values |

**NumericDoubleValidationSchema Attributes (extends Schema):**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| minValue | float | N | Minimum allowed values |
| maxValue | float | N | Maximum allowed values |
| minPrecision | integer | N | Minimum precision levels values must have to be valid |
| maxPrecision | integer | N | Maximum precision levels values must have to be valid |

**SelectValidationSchema Attributes (extends Schema):**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| multiselect | bool | N | If true, indicates multiple values can be selected. If multiple values are selected, resulting values are arrays of selected values. Default is false |
| options | []string | Y | Lists of acceptable options users can select from. Data types for each option must match parameter setting data types |

**ComponentProperties for helm.v3:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| repository | string | Y | URLs indicating Helm chart locations |
| revision | string | Y | Helm chart full versions |
| wait | bool | N | If True, indicates devices MUST wait until Helm charts finish installing before installing next Helm charts. Default is True. Workload Fleet Management Client MUST support True and MAY support False. Only applies if multiple helm.v3 components are provided. |
| timeout | string | N | Time to wait for component installation completion. If installation does not complete before timeout occurs, the installation process fails. Format is "##m##s" indicating total number of minutes and seconds to wait. |

**ComponentProperties for compose:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| packageLocation | string | Y | URLs indicating Compose package locations |
| keyLocation | string | N | Public keys used to validate digitally signed packages. It is highly recommended to digitally sign packages. When signing packages, PGP MUST be used |
| wait | bool | N | If True, indicates devices MUST wait until Compose files finish starting up before starting next Compose files. Default is True. Workload Fleet Management Client MUST support True and MAY support False. Only applies if multiple compose components are provided. |
| timeout | string | N | Time to wait for component installation completion. If installation does not complete before timeout occurs, the installation process fails. Format is "##m##s" indicating total number of minutes and seconds to wait. |

> **Investigation Needed:** We need to have more discussion about how Compose should be handled and what is required here.

## Application Description Examples

### Example 1: Simple Application Description

A simple hello-world example of an ApplicationDescription:

```yaml
apiVersion: margo.org/v1-alpha1
kind: ApplicationDescription
metadata:
  id: com-northstartida-hello-world
  name: Hello World
  description: A basic hello world application
  version: "1.0"
  catalog:
    application:
      icon: ./resources/hw-logo.png
      tagline: Northstar Industrial Application's hello world application.
      descriptionFile: ./resources/description.md
      releaseNotes: ./resources/release-notes.md
      licenseFile: ./resources/license.pdf
      site: http://www.northstar-ida.com
      tags: ["monitoring"]
    author:
      - name: Roger Wilkershank
        email: rpwilkershank@northstar-ida.com
    organization:
      - name: Northstar Industrial Applications
        site: http://northstar-ida.com
deploymentProfiles:
  - type: helm.v3
    components:
      - name: hello-world
        properties:
          repository: oci://northstarida.azurecr.io/charts/hello-world
          revision: 1.0.1
          wait: true
parameters:
  greeting:
    value: Hello
    targets:
      - pointer: global.config.appGreeting
        components: ["hello-world"]
  greetingAddressee:
    value: World
    targets:
      - pointer: global.config.appGreetingAddressee
        components: ["hello-world"]
configuration:
  sections:
    - name: General Settings
      settings:
        - parameter: greeting
          name: Greeting
          description: The greeting to use.
          schema: requireText
        - parameter: greetingAddressee
          name: Greeting Addressee
          description: The person, or group, the greeting addresses.
          schema: requireText
  schema:
    - name: requireText
      dataType: string
      maxLength: 45
      allowEmpty: false
```

### Example 2: Complex Application Description with Multiple Deployment Profiles

An example ApplicationDescription defining deployment profiles for both Helm and Compose:

```yaml
apiVersion: margo.org/v1-alpha1
kind: ApplicationDescription
metadata:
  id: com-northstartida-digitron-orchestrator
  name: Digitron orchestrator
  description: The Digitron orchestrator application
  version: 1.2.1
  catalog:
    application:
      icon: ./resources/ndo-logo.png
      tagline: Northstar Industrial Application's next-gen, AI driven, Digitron instrument orchestrator.
      descriptionFile: ./resources/description.md
      releaseNotes: ./resources/release-notes.md
      licenseFile: ./resources/license.pdf
      site: http://www.northstar-ida.com
      tags: ["optimization", "instrumentation"]
    author:
      - name: Roger Wilkershank
        email: rpwilkershank@northstar-ida.com
    organization:
      - name: Northstar Industrial Applications
        site: http://northstar-ida.com
deploymentProfiles:
  - type: helm.v3
    components:
      - name: database-services
        properties:
          repository: oci://quay.io/charts/realtime-database-services
          revision: 2.3.7
          wait: true
          timeout: 8m30s
      - name: digitron-orchestrator
        properties:
          repository: oci://northstarida.azurecr.io/charts/northstarida-digitron-orchestrator
          revision: 1.0.9
          wait: true
  - type: compose
    components:
      - name: digitron-orchestrator-docker
        properties:
          packageLocation: https://northsitarida.com/digitron/docker/digitron-orchestrator.tar.gz
          keyLocation: https://northsitarida.com/digitron/docker/public-key.asc
parameters:
  idpName:
    targets:
      - pointer: idp.name
        components: ["digitron-orchestrator"]
      - pointer: ENV.IDP_NAME
        components: ["digitron-orchestrator-docker"]
  idpProvider:
    targets:
      - pointer: idp.provider
        components: ["digitron-orchestrator"]
      - pointer: ENV.IDP_PROVIDER
        components: ["digitron-orchestrator-docker"]
  idpClientId:
    targets:
      - pointer: idp.clientId
        components: ["digitron-orchestrator"]
      - pointer: ENV.IDP_CLIENT_ID
        components: ["digitron-orchestrator-docker"]
  idpUrl:
    targets:
      - pointer: idp.providerUrl
        components: ["digitron-orchestrator"]
      - pointer: idp.providerMetadata
        components: ["digitron-orchestrator"]
      - pointer: ENV.IDP_URL
        components: ["digitron-orchestrator-docker"]
  adminName:
    targets:
      - pointer: administrator.name
        components: ["digitron-orchestrator"]
      - pointer: ENV.ADMIN_NAME
        components: ["digitron-orchestrator-docker"]
  adminPrincipalName:
    targets:
      - pointer: administrator.userPrincipalName
        components: ["digitron-orchestrator"]
      - pointer: ENV.ADMIN_PRINCIPALNAME
        components: ["digitron-orchestrator-docker"]
  pollFrequency:
    value: 30
    targets:
      - pointer: settings.pollFrequency
        components: ["digitron-orchestrator", "database-services"]
      - pointer: ENV.POLL_FREQUENCY
        components: ["digitron-orchestrator-docker"]
  siteId:
    targets:
      - pointer: settings.siteId
        components: ["digitron-orchestrator", "database-services"]
      - pointer: ENV.SITE_ID
        components: ["digitron-orchestrator-docker"]
  cpuLimit:
    value: 1
    targets:
      - pointer: settings.limits.cpu
        components: ["digitron-orchestrator"]
  memoryLimit:
    value: 16384
    targets:
      - pointer: settings.limits.memory
        components: ["digitron-orchestrator"]
configuration:
  sections:
    - name: General
      settings:
        - parameter: pollFrequency
          name: Poll Frequency
          description: How often the service polls for updated data in seconds
          schema: pollRange
        - parameter: siteId
          name: Site Id
          description: Special identifier for the site (optional)
          schema: optionalText
    - name: Identity Provider
      settings:
        - parameter: idpName
          name: Name
          description: The name of the Identity Provider to use
          immutable: true
          schema: requiredText
        - parameter: idpProvider
          name: Provider
          description: Provider something something
          immutable: true
          schema: requiredText
        - parameter: idpClientId
          name: Client ID
          description: The client id
          immutable: true
          schema: requiredText
        - parameter: idpUrl
          name: Provider URL
          description: The url of the Identity Provider
          immutable: true
          schema: url
    - name: Administrator
      settings:
        - parameter: adminName
          name: Presentation Name
          description: The presentation name of the administrator
          schema: requiredText
        - parameter: adminPrincipalName
          name: Principal Name
          description: The principal name of the administrator
          schema: email
    - name: Resource Limits
      settings:
        - parameter: cpuLimit
          name: CPU Limit
          description: Maximum number of CPU cores to allow the application to consume
          schema: cpuRange
        - parameter: memoryLimit
          name: Memory Limit
          description: Maximum number of memory to allow the application to consume
          schema: memoryRange
  schema:
    - name: requiredText
      dataType: string
      maxLength: 45
      allowEmpty: false
    - name: email
      dataType: string
      allowEmpty: false
      regexMatch: .*@[a-z0-9.-]*
    - name: url
      dataType: string
      allowEmpty: false
      regexMatch: ^(http(s):\/\/.)[-a-zA-Z0-9@:%._\+~#=]{2,256}\.[a-z]{2,6}\b([-a-zA-Z0-9@:%_\+.~#?&//=]*)$
    - name: pollRange
      dataType: integer
      minValue: 30
      maxValue: 360
      allowEmpty: false
    - name: optionalText
      dataType: string
      minLength: 5
      allowEmpty: true
    - name: cpuRange
      dataType: double
      minValue: 0.5
      maxPrecision: 1
      allowEmpty: false
    - name: memoryRange
      dataType: integer
      minValue: 16384
      allowEmpty: false
```

## Device API

### Device Capabilities

Devices MUST provide workload orchestration services with capabilities and characteristics using Device API device capabilities endpoints.

**Route and HTTP Methods:**
- `POST /device/{deviceId}/capabilities`
- `PUT /device/{deviceId}/capabilities`

**Route Parameters:**

| Parameter | Type | Required? | Description |
|-----------|------|-----------|-------------|
| {deviceId} | string | Y | Device IDs registered with workload orchestration web services during onboarding |

**Request Body Fields:**

| Field | Type | Required? | Description |
|-------|------|-----------|-------------|
| apiVersion | string | Y | Identifier of API version followed by object definition |
| kind | string | Y | Must be DeviceCapabilities |
| properties | Properties | Y | Elements defining device characteristics |

**Properties Fields:**

| Field | Type | Required? | Description |
|-------|------|-----------|-------------|
| id | string | Y | Unique deviceIDs assigned to devices via Device Owners |
| vendor | string | Y | Defines device vendors |
| modelNumber | string | Y | Defines device model numbers |
| serialNumber | string | Y | Defines device serial numbers |
| roles | []string | Y | Elements defining device roles they can provide. MUST be one of: Standalone Cluster, Cluster Leader, or Standalone Device |
| resources | []Resource | Y | Elements defining device resources available to applications |
| peripherals | []Peripheral | Y | Elements defining device peripherals available to applications |
| interfaces | []Interface | Y | Elements defining device interfaces available to applications |

**Example Request:**

```json
{
  "apiVersion": "device.margo/v1",
  "kind": "DeviceCapability",
  "properties": {
    "id": "northstarida.xtapro.k8s.edge",
    "vendor": "Northstar Industrial Applications",
    "modelNumber": "332ANZE1-N1",
    "serialNumber": "PF45343-AA",
    "roles": ["standalone cluster", "cluster lead"],
    "resources": {
      "memory": "64.0 GB",
      "storage": "2000 GB",
      "cpus": [{
        "architecture": "Intel x64",
        "cores": 24,
        "frequency": "6.2 GHz"
      }]
    },
    "peripherals": [{
      "name": "NVIDIA GeForce RTX 4070 Ti SUPER OC Edition Graphics Card",
      "type": "GPU",
      "modelNumber": "TUF-RTX4070TIS-O16G",
      "properties": {
        "manufacturer": "NVIDIA",
        "series": "NVIDIA GeForce RTX 40 Series",
        "gpu": "GeForce RTX 4070 Ti SUPER",
        "ram": "16 GB",
        "clockSpeed": "2640 MHz"
      }
    }],
    "interfaces": [
      {
        "name": "RTL8125 NIC 2.5G Gigabit LAN Network Card",
        "type": "Ethernet",
        "modelNumber": "RTL8125",
        "properties": {
          "maxSpeed": "2.5 Gbps"
        }
      }
    ]
  }
}
```

### Deployment Status

While applying new desired states, device management clients MUST provide workload orchestration web services with current status indications using Device API device status endpoints.

**Route and HTTP Methods:**
- `POST /device/{deviceId}/deployment/{deploymentId}/status`

**Route Parameters:**

| Parameter | Type | Required? | Description |
|-----------|------|-----------|-------------|
| {deviceId} | string | Y | Device IDs registered with workload orchestration solutions during onboarding |
| {deploymentId} | string | Y | Deployment IDs for which status is being reported |

**Request Body Fields:**

| Field | Type | Required? | Description |
|-------|------|-----------|-------------|
| apiVersion | string | Y | Identifier of API version followed by object definition |
| kind | string | Y | Must be DeploymentStatus |
| deploymentId | string | Y | Unique identifier UUIDs of deployment specifications |
| status | []status | Y | Elements defining overall deployment status |
| components | []components | Y | Elements defining individual component deployment status |

**Status Fields:**

| Field | Type | Required? | Description |
|-------|------|-----------|-------------|
| state | string | Y | Current overall deployment states. MUST be one of: Pending, Installing, Installed, Failed |
| error | Error | N | Elements defining overall installation errors if they occurred |

**Example Request:**

```json
{
  "apiVersion": "deployment.margo/v1",
  "kind": "DeploymentStatus",
  "deploymentId": "a3e2f5dc-912e-494f-8395-52cf3769bc06",
  "status": {
    "state": "pending",
    "error": {
      "code": "",
      "message": ""
    }
  },
  "components": [
    {
      "name": "digitron-orchestrator",
      "state": "pending",
      "error": {
        "code":"",
        "message":""
      }
    }
  ]
}
```

## Onboarding API

### Device Onboarding

**Route and HTTP Methods:**
- `POST /onboarding/`

Note: This API requires definition with specific request and response body specifications.

### Certificate API (Root CA Download)

To facilitate secure communication between device management clients and workload orchestration web services, workload orchestration web service root CA certificates must be downloaded using Onboarding API certificate endpoints.

**Route and HTTP Methods:**
- `GET /onboarding/certificate`

**Response Body:**

```json
{
  "certificate": "<base-64 encoded certificate text>"
}
```

# Security Requirements

## Authentication and Authorization

All API communications MUST use bearer token authentication with proper certificate-based payload signing.

## Device Security

Devices MUST implement:
- TPM-based hardware security
- Secure boot processes
- Attestation mechanisms
- Zero Trust Network Access (ZTNA)

## Communication Security

All communications between components MUST use:
- TLS encryption
- Certificate-based authentication
- Payload signing and verification
- Industry-standard security protocols

# Compliance and Conformance

## Compliance Requirements

To achieve Margo compliance, implementations MUST:
- Support all mandatory [API endpoints](#api-reference)
- Implement required [security mechanisms](#security-requirements)
- Pass compliance test suite validation
- Support specified application packaging formats

## Conformance Testing

The Margo compliance test suite validates:
- API compatibility
- Security implementation
- Application package handling
- Device onboarding processes
- Observability data collection

# Implementation Guidelines

## Contributing to Margo

### General Requirements

Contributions to Margo are typically welcome! However, when adding new enhancements, maintainers must balance added value against increased maintenance costs. Creating new issues on GitHub before beginning implementation and awaiting maintainer feedback is recommended.

### Bug Fixes

Bug and security fixes are always welcome and receive the highest priority.

### Contribution Checklist

- Contributions to the Specification must be covered by Corporate CLA or Individual CLA
- Code changes must be accompanied by automated tests
- Add required copyright headers to each new file introduced when appropriate
- Add `signed-off` to all commits to certify the "Developer's Certificate of Origin"
- Structure commits logically in small steps
- Base commits on the latest `pre-draft` branch

### Signing the CLA

If you have not signed the Individual CLA, or your organization has not signed the Corporate CLA, the LFX EasyCLA bot will prompt you to follow appropriate steps to authorize your contribution.

### Developer's Certificate of Origin

By making a contribution to this project, I certify that:

(a) The contribution was created in whole or in part by me and I have the right to submit it under the open source license indicated in the file; or

(b) The contribution is based upon previous work that, to the best of my knowledge, is covered under an appropriate open source license and I have the right under that license to submit that work with modifications, whether created in whole or in part by me, under the same open source license (unless I am permitted to submit under a different license), as indicated in the file; or

(c) The contribution was provided directly to me by some other person who certified (a), (b) or (c) and I have not modified it.

(d) I understand and agree that this project and the contribution are public and that a record of the contribution (including all personal information I submit with it, including my sign-off) is maintained indefinitely and may be redistributed consistent with this project or the open source license(s) involved.

## Best Practices

- Implement comprehensive logging and monitoring
- Follow secure development practices
- Use industry-standard containerization technologies
- Implement proper error handling and recovery
- Design for offline and intermittent connectivity scenarios

## Specification Maintenance and Consistency

### Recommendations for Multi-Platform Specification Management

**1. Sync Missing Content**: The GitHub specification should include the detailed investigation notes, action items, and extended examples found on the website.

**2. Maintain Consistency**: Ensure that schema definitions, API examples, and YAML structures match between both versions.

**3. Add Website-Specific Content**: Consider adding the more detailed application description examples and extended schema validation rules found on the website to the GitHub specification.

**4. Preserve Contextual Information**: The "Action:" and "Investigation Needed:" notes from the website provide valuable context about ongoing work and should be preserved in the GitHub version.

## Documentation Generation Process

Some resources used in Margo APIs are manually specified using Markdown files. In this case, mkdocs generates HTML format documentation.

For others, the modeling language LinkML is used to generate documentation. In this case, linkml generates Markdown documents integrated with the aforementioned documents before mkdocs generates HTML documents.

---

## Copyright and License

Copyright © 2024 Margo

This specification is licensed under [appropriate open source license].

---

**Disclaimer:** This is a work-in-progress draft specification. Do not attempt to implement this version or reference it as authoritative. The specification is subject to change without notice.
