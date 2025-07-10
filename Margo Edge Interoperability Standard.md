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

The Margo Edge Interoperability Standard addresses the growing complexity of managing heterogeneous edge computing environments in industrial automation. As manufacturing facilities increasingly deploy compute devices and applications from multiple vendors, organizations face significant challenges in lifecycle management, workload orchestration, and operational scalability. This specification defines a comprehensive framework for achieving seamless interoperability between edge devices, applications, and fleet management systems through standardized APIs, security protocols, and deployment mechanisms. By establishing common approaches for application packaging, device onboarding, workload observability, and fleet management, Margo enables organizations to deploy, scale, and operate complex multi-vendor edge environments efficiently while reducing integration barriers and accelerating digital transformation initiatives. This open standard, hosted by the Linux Foundation, provides the technical foundation for building interoperable industrial edge ecosystems that support both current and future technological requirements.

## What is Margo?

The Margo initiative is an open collaboration between like-minded organizations and individuals, sharing a common vision: deliver edge interoperability for industrial automation ecosystems. Margo is hosted by the Linux Foundation.

## Mission Statement

Margo's mission emphasizes unlocking innovation barriers in industrial automation through edge interoperability. This is achieved through creation of a reference implementation, open standard, and compliance testing toolkit to facilitate the interoperable orchestration of edge applications and devices. The Margo initiative aims to accelerate digital transformation by simplifying the deployment, scalability, and operation of industrial solutions, thereby enabling organizations to innovate and grow more efficiently.

## Addressing Industrial Edge Pain Points

Because of recent trends in industrial manufacturing, lifecycle management is becoming a challenge because of the massive increase of compute devices and apps from a multitude of suppliers deployed in plants. Margo intends to reduce the complexity of maintaining and operating such an infrastructure at scale in a multi-vendor environment.

## Applying an Open Approach

While Margo intends to address innovation barriers, this cannot be done without widespread collaboration and interaction with peer communities.

## Core Deliverables

The core deliverables for Margo address the mission to enable interoperability at scale:
- **Reference Implementation**
- **Open Standard** 
- **Open Compliance Test Suite**

# System Architecture Overview

## Envisioned System Design

Margo intends to create an open [interoperability](#terms-and-definitions) standard and ecosystem for the industrial edge, allowing [edge compute devices](#edge-compute-device), [workloads](#workload), and [fleet management](#fleet-management) software to be compatible and interoperable across manufacturers and software developers willing to adopt such standard.

<img width="912" height="677" alt="image" src="https://github.com/user-attachments/assets/1a002916-4dc8-453b-a23c-af9bb47ad20d" />

The envisioned system can be broken down into the following main components:

### Workloads
Workloads are the software deployed to Margo-compliant edge compute devices. These are containerized applications packaged using standardized methods that enable deployment across multiple compatible edge devices while maintaining consistency and reliability. See the [Application Interoperability](#application-interoperability) section to learn more about Margo's supported workloads and how they are packaged.

### Workload Observability
For distributed systems it's vitally important to collect diagnostics information about the workloads and systems running within the environment. Margo makes use of the OpenTelemetry specification to capture this information, providing comprehensive monitoring capabilities across the edge ecosystem. See the [Workload Observability](#workload-observability) section to see how Margo is making use of the OpenTelemetry specification to capture this information.

### Workload Fleet Management
Workload fleet management software is the centralized software solution for managing the lifecycle of workloads on Margo compliant edge compute devices. This component provides orchestration, deployment, and management capabilities across distributed edge environments. See the [Fleet Management](#fleet-management) section to learn more.

### Edge Compute Devices
Edge compute devices are the compute surfaces workloads are deployed to and run on. As part of the Margo initiative, we are very prescriptive about how edge compute devices must be configured to make them Margo compliant, ensuring guaranteed functionality and interoperability. See the [Device Interoperability](#device-interoperability) section to learn more.

## Software Composition Overview

Applications can be found in completely different stages:
- **"Application Packaging":** application has been prepared and made ready for deployment
- **"Application Deployment" (AKA Runtime):** application has been made available and accessible on the device

Note: Logically there is another intermediate stage: "Application Staging". This is the stage in which the application is set up, configured, and made available for use to the device, but has not yet been deployed (started) to be used. As of now this stage is out of scope in the Margo specification and is covered in detail in the [Software Composition](#software-composition) section.

## Device Roles

Supported Device roles:
- **Standalone Cluster** (Leader and/or Worker)
- **Cluster Leader**
- **Cluster Worker** 
- **Standalone Device**

Note: Additional device roles will be introduced as the specification matures. Details are provided in the [Device Interoperability](#device-interoperability) section.

## Margo Device Layers

Margo device roles consist of three major layers:
- **Margo Interface Layer**
- **Platform Layer**
- **Traditional Device Layer**

<img width="678" height="430" alt="image" src="https://github.com/user-attachments/assets/6a0690c5-6dfb-43b9-b386-e54946fc057c" />

Although Margo requires compliance towards its requirements, such as hosting the Margo management interface client, the device vendor has freedom to implement as they see fit. Complete details are available in the [Device Interoperability](#device-interoperability) section.

# Scope

This specification defines the technical requirements and interfaces for achieving edge interoperability in industrial automation environments. It covers:

- Application packaging and deployment standards
- Device management and orchestration interfaces
- Fleet management protocols and APIs
- Workload observability and monitoring requirements
- Security and authentication mechanisms
- Compliance testing frameworks

# Normative References

The following documents are referred to in this specification:
- ISO/IEC 20922:2016 (MQTT)
- OCI Container Specification
- Kubernetes API Specification
- Helm Chart Specification v3
- Docker Compose Specification
- OpenTelemetry Specification
- OpenGitOps Specification

# Terms and Definitions

## Core Concepts

**Interoperability:** For Margo, interoperability is about achieving the following:
- Defining a common approach for packaging [Components](#component) so they can be deployed as [Workloads](#workload) to any compatible Margo-compliant [Edge Compute Devices](#edge-compute-device) via any Margo-compliant [Workload Fleet Manager](#workload-fleet-manager)
- Defining a common approach for packaging device software and firmware updates so they can be deployed to any Margo-compliant [Edge Compute Devices](#edge-compute-device) via any Margo-compliant [Device Fleet Manager](#device-fleet-manager)
- Defining a common API to enable communication between any Margo-compliant [Edge Compute Devices](#edge-compute-device) and any Margo-compliant [fleet management](#fleet-management) software
- Defining a common approach for collecting and transmitting diagnostics and observability data from a Margo-compliant [Edge Compute Device](#edge-compute-device)

**Orchestration:** For Margo, orchestration is about the deployment of [Workloads](#workload) and device software updates to Margo-compliant [Edge Compute Devices](#edge-compute-device) via Margo-compliant [fleet management](#fleet-management) software. Margo depends on container orchestration platforms such as Kubernetes, Docker and Podman existing on the [Edge Compute Devices](#edge-compute-device) and is not an attempt to duplicate what these platforms provide.

**Fleet Management:** Fleet Management represents a concept or pattern that enables users to manage one to many set of [workloads](#workload) and devices the customer owns. Many strategies exist within fleet management such as canary deployments, rolling deployments, and many others.

**State Seeking:** The state seeking methodology, adopted via Margo, is enabled first by the [Workload Fleet Manager](#workload-fleet-manager) when it establishes the "Desired state". The [Edge Device](#edge-compute-device) then reconciles its "Current state" with the "Desired state" provided by the Fleet Manager and reports the status.

**Provider Model:** The provider model within Margo describes a service that is able to orchestrate or implement the desired state within the [Edge Device](#edge-compute-device). Current providers supported: Helm Client, Compose Client.

## Technical Terms

**Application:** An application is a collection of one, or more, [Components](#component), as defined by an Application Description, and bundled within an [application package](#application-package).

**Application Package:** An Application Package is used to distribute an [application](#application). According to the specification, it is a folder in a Git repository referenced by URL, which contains the Application Description (that refers to contained and deployable [Components](#component)) as well as associated resources (e.g., icons).

**Component:** A Component is a piece of software tailored to be deployed within a customer's environment on an [Edge Compute Device](#edge-compute-device). Currently Margo-supported components are:
- Helm Chart
- Compose Archive

**Workload:** A Workload is an instance of a [Component](#component) running within a customer's environment on an [Edge Compute Device](#edge-compute-device).

**Edge Compute Device:** Edge Compute Devices are represented by compute hardware that runs within the customer's environment to enable the system with Margo Compliant [Workloads](#workload). Edge Compute Devices host the Margo compliant management agents, container orchestration platform, and device operating systems. Margo Edge Compute Devices are defined by the roles they can facilitate within the Margo Architecture.

**Workload Fleet Manager:** Workload Fleet Manager (WFM) represents a software offering that enables End Users to configure, deploy, and manage edge [Workloads](#workload) as a fleet on their registered [Edge Devices](#edge-compute-device).

**Workload Fleet Management Client:** The Workload Fleet Management Agent is a service that runs on the [Edge Compute Device](#edge-compute-device) which communicates with the [Workload Fleet Manager](#workload-fleet-manager) to receive [Components](#component) that will be instantiated as [Workloads](#workload) and configurations to be applied on the [Edge Compute Device](#edge-compute-device).

**Device Fleet Manager:** Device Fleet Manager (DFM) represents a software offering that enables End Users to onboard, delete, and maintain [Edge Compute Devices](#edge-compute-device) within the ecosystem. This software is utilized in conjunction with the [Workload Fleet Manager](#workload-fleet-manager) software to provide users with the features required to manage their [Edge Device](#edge-compute-device) along with [Workloads](#workload) running on them.

**Device Fleet Management Client:** The Device Fleet Management Agent is a service that runs on the [Edge Compute Device](#edge-compute-device) which communicates with the [Device Fleet Manager](#device-fleet-manager) to receive device configuration to be applied on the [Edge Compute Device](#edge-compute-device).

**Application Registry:** An Application Registry holds [Application](#application) Packages. It is used by developers to make their applications available. An Application Registries MUST be a Git repository. [Workload Fleet Managers](#workload-fleet-manager) cannot access Application Registries directly, they can only access [Application Catalogs](#application-catalog).

**Application Catalog:** An Application Catalog holds [Application](#application) Packages that were preselected to be install-ready for the edge environment of a [Workload Fleet Manager](#workload-fleet-manager) to deploy them to managed [Edge Compute Devices](#edge-compute-device). An Application Catalog obtains the offered [Applications](#application) from one or more [Application Registries](#application-registry).

**Component Registry:** A Component Registry holds [Components](#component) (e.g., Helm Charts and Compose Archives) for Application Packages. When an application gets deployed through a [Workload Fleet Manager](#workload-fleet-manager), the components (linked within an Application Description) are requested from the Component Registry. This can be implemented, for example, as an OCI Registry.

**Workload Marketplace:** Workload Marketplace is the location where end users purchase the rights to access [Workloads](#workload) from a vendor. Functional Requirements include: providing users with a list of [Workloads](#workload) available for purchase, enabling users to purchase access rights to a [Workload](#workload), and enabling users with the meta data to access associated Workload Registries/Repositories. Note: The Workload Marketplace component is out of scope for Project Margo.

# Personas and Use Cases

## Persona Categories

<img width="1053" height="354" alt="image" src="https://github.com/user-attachments/assets/9c91a9de-7389-485d-bfbb-e2c229c6c7cc" />

### End Users
- Evaluates the best-in-class products offered by the suppliers
- Consumes products provided by the suppliers
- Assembles multiple vendor's products into a productive automation system

### Suppliers
- Contribute knowledge and expertise to the specification sections relevant to their business
- Builds products compliant with the Margo specification
- Markets and sells their products to end users

## Detailed Persona Definitions

### End User Personas

**OT User**
Consumer of functionality provided by the application vendors to run critical and non-critical business functions, or to improve business efficiency

**OT Architect**
Creates and enforces standards across OT deployment locations or sites for greater supportability and consistency

**Integrator**
Optional persona, external to the organization of the other end user personas, but tasked with assembling and installing hardware and software provided by the suppliers

**IT Service Provider**
Provides "IT-like" services, such as connectivity, backup and restore, automation, security and auditing, within the End User's OT environment

### Supplier Personas

**Workload Supplier**
Provides an application that performs some desired function, such as computer vision, software-defined control, etc, which is deployed to device via a [Workload Fleet Manager](#workload-fleet-manager)

**Fleet Management Supplier**
Provides a software package that enables End Users to manage their [workloads](#workload) and/or devices the [workloads](#workload) run on via [Fleet management](#fleet-management) patterns.

**Device Supplier**
Provides hardware resources, such as CPU and memory, along with lifecycle support, such as firmware and BIOS updates

**Platform Supplier**
Provides operating system level software to abstract hardware resources, and optionally, container orchestration software on top of the operating system layer

## Multiple Personas per Entity

![image](https://github.com/user-attachments/assets/59a59b28-9c78-4260-b80f-6ecfecc33216)

In some cases, a single entity, such as a company or organization, may provide multiple roles within the Margo enabled ecosystem:

### Combination of Device Supplier and Platform Supplier
A supplier may be both the device supplier (the underlying hardware) and the platform provider, selling a "ready to deploy" offering for consumption by end users.

### Combination of Workload Supplier and OT User
A persona in the end user category, such as an OT user, could also internally develop an application, and leverage the Margo specification to ease deployment to devices in their environment.

# Software Composition

## Terminology Scoping

This section scopes terminology to the software packaging and deployment stages:

**Application:** The term [application](#application) applies to all stages.

**Workload:** The term [Workload](#workload) applies only to running software (deployment stage). [Workloads](#workload) are the result of deploying [Components](#component).

**Component:** The term [Component](#component) applies to the resources available in packaged software that get deployed as [Workloads](#workload). Some [providers](#provider-model) might support that multiple [Workload](#workload) replicas are deployed from a single [Component](#component). [Components](#component) are made available over [Component Registries](#component-registry).

Components might have different shapes depending on their type and stage:
- **Helm v3 as [Component](#component):** a Helm Chart
- **Helm v3 as [Workload](#workload):** all container images required by the to-be-started pods
- **Compose as [Component](#component):** a Compose Archive
- **Compose as [Workload](#workload):** a Compose file and all the container images required by the to-be-started services

## Software Packaging Stage

![image](https://github.com/user-attachments/assets/03b578bf-180e-4f72-a776-bb0fc4baf443)

Software at rest is made available as an [Application Package](#application-package), which is a folder with a Margo-defined structure comprising the software application. This [Application Package](#application-package) contains:

- An Application Description that is a Margo-specific way to define the composition of one or more [Components](#component). These [Components](#component) are linked in the Application Description document, are deployable as [workloads](#workload), and are provided in a Margo-supported way, e.g. as Helm Charts or Compose Archives.
- Some application resources: icon, license(s), release notes

[Application Packages](#application-package) and [Components](#component) are managed and hosted separately:
- [Application Registries](#application-registry) store Application Descriptions and their associated application resources. An [Application Registry](#application-registry) is implemented as a git repository.
- [Component Registries](#component-registry) store [Components](#component)

The following diagram shows, at hand of an example, the relationship between an Application Package and the Components listed within its Deployment Profiles:

![image](https://github.com/user-attachments/assets/fe7f7876-9f09-4abf-854b-b86cde441a9d)

The following diagram is similar to the previous one, but showing more details on the Component Registry for a Compose Deployment Profile:

![image](https://github.com/user-attachments/assets/d4aa7979-f877-4b85-bf23-8960b3e53673)

The application and contained components are typically configurable with the option of providing default values.

## Software Deployment Stage

When a device gets the instruction to run an [Application](#application) (over a desired-state specified with an ApplicationDeployment object), its [Workload Fleet Management Agent](#workload-fleet-management-client) interacts with the [providers](#provider-model). That way all [Workloads](#workload) needed for an [Application](#application) should get started and the desired state should be reached.

![image](https://github.com/user-attachments/assets/89643a6a-1003-4845-9214-104d8a218b22)

In this stage the [providers](#provider-model) are responsible for managing the individual [Workloads](#workload).

On a Helm v3 Deployment Profile, a [Workload Fleet Management Agent](#workload-fleet-management-client) implementation could utilize the Helm API to start the individual Helm Charts.

On a Compose Deployment Profile, a [Workload Fleet Management Agent](#workload-fleet-management-client) implementation could utilize the Compose CLI to start the individual [Workloads](#workload).

The following diagram shows the result of reaching the desired state for an Application with a Helm v3 Deployment Profile (the result of helm install).

![image](https://github.com/user-attachments/assets/9518af5c-ca2d-4614-90ef-aff9179d27ff)

The following diagram shows the result of reaching the desired state for an Application with a Compose Deployment Profile (the result of the compose up CLI call).

![image](https://github.com/user-attachments/assets/d8578c95-2304-4265-aab5-35271502b634)

# Application Interoperability

## Workloads Overview

A [workload](#workload) is software deployed to, and run on, Margo compliant [edge compute devices](#edge-compute-device).

In order to help achieve Margo's [interoperability](#terms-and-definitions) mission statement, we are initially targeting containerized [workloads](#workload) capable of running on platforms like Kubernetes, Docker and Podman. The flexibility these platforms provide enables [workload suppliers](#workload-supplier) to define and package their [workloads](#workload) in a common way using Helm or the Compose specification so they can more easily be deployed to multiple compatible [edge compute devices](#edge-compute-device).

While Margo is initially targeting deployments using Helm or Compose, we plan to support other deployment types in the future. One of our design goals is to make it easier for [workload fleet managers](#workload-fleet-manager) to support the current and future deployment types without having to implement special logic for each type.

## Application Description Model Goals

The three main goals of Margo's application description model is to allow [workload fleet managers](#workload-fleet-manager) to do the following:
- Display information about the [workloads](#workload) the [OT user](#personas-and-use-cases) can deploy (e.g., a [workload catalog](#workload-marketplace))
- Determine which [edge compute devices](#edge-compute-device) are compatible with the [workloads](#workload) (e.g., processor types match, GPU present, etc.)
- Capture, and validate, configuration information from the [OT user](#personas-and-use-cases) when deploying and updating [workloads](#workload)

Another advantage of Margo's application description model is to enable [workload suppliers](#workload-supplier) to define different deployment profiles in a single application description file to target deploying to different types of [edge compute devices](#edge-compute-device) (e.g., Arm vs. x86, Kubernetes vs. Docker) instead of needing to maintain multiple application description files.

## Packaging

To distribute one, or more, [workloads](#workload) they are wrapped in an [application package](#application-package) that is provided by the [workload supplier](#workload-supplier) who aims to provide it to Margo-compliant [edge compute devices](#edge-compute-device). Therefore, the [workload supplier](#workload-supplier) creates an application description YAML document containing information about the application and a reference on how to deploy the OCI-containerized [workloads](#workload) that make up the application. The [application package](#application-package) is made available in an [application registry](#application-registry) and the OCI artifacts are stored in a remote or [local registry](#local-registries).

**Example workflow**

The following diagram provides an example workflow showing one way a workload fleet manager might use the application description information:

<img width="844" height="542" alt="image" src="https://github.com/user-attachments/assets/9612134e-4a7f-4c1c-a2af-91e71b64ac94" />

- An end user visits an workload catalog of the Workload Fleet Manager Frontend.
- This frontend requests all workloads from the Workload Fleet Manager.
- Either: the Workload Fleet Manager requests all application descriptions from each known Application Registry.
- Or: the Workload Fleet Manager maintains a cache of application descriptions and services the request from there.
- The Workload Fleet Manager returns the retrieved documents of application descriptions to the frontend.
- The frontend parses the metadata element of all received application description documents.
- The frontend presents the parsed metadata in a UI to the end user.
- The end user selects the workload to be installed.
- The frontend parses the configuration element of the selected application description.
- The frontend presents the parsed configuration to the user.
- The end user fills out the configurable application parameters to be applied to the workload.
- The frontend creates an ApplicationDeployment definition (from the ApplicationDescription and the filled out parameters) and sends it to the Workload Fleet Manager, which executes it as the desired state.

## Application Package Definition

The [application package](#application-package) comprises the following elements:

- **Application Description:** A YAML document with the element `kind` defined as `ApplicationDescription`, stored in a file (e.g., `margo.yaml`) containing metadata, deployment configurations, and configurable parameters. There SHALL be only one YAML file in the package root of kind `ApplicationDescription`.
- **Resources:** Additional information about the application (e.g., manual, icon, release notes, license file) that can be provided in an [application catalog](#application-catalog) or [marketplace](#workload-marketplace).

## Package Structure

```
/                           # REQUIRED top-level directory 
└── application description # REQUIRED a YAML document with element 'kind' as 'ApplicationDescription' stored in a file  (e.g., 'margo.yaml')
└── resources               # OPTIONAL folder with application resources (e.g., icon, license file, release notes) that can be displayed in an application catalog
```

> Note
> Application catalogs or marketplaces are out of scope for Margo. The exact requirements of the marketing material shall be defined by the application marketplace beyond outlined mandatory content.

## Deployment Profiles

The deployment profiles specified in the application description SHALL be defined as Helm Charts AND/OR Compose components:

- **For Kubernetes devices:** Applications must be packaged as Helm charts using Helm (version 3)
- **For Compose devices:** Applications must be packaged as a tarball file containing the compose.yml file and any additional artifacts referenced by the Compose file. It is highly recommended to digitally sign this package. When digitally signing the package PGP encryption MUST be used.

> Investigation Needed: We plan to do a security review of this package definition later. During this review we will revisit the way the Compose tarball file should be > > > signed. We will also discuss how we should handle secure container registries that require a username and password.
> 
> Investigation Needed: We need to determine what impact, if any, using 3rd party helm charts has on being Margo compliant.
> 
> Investigation Needed: Missing in the current specification are ways to define the compatibility information (resources required to run, application dependencies) as well as required infrastructure services such as storage, message queues/bus, reverse proxy, or authentication/authorization/accounting.

If either one cannot be implemented it MAY be omitted but Margo RECOMMENDS defining deployment profiles as both Helm chart AND Compose components to strengthen interoperability and applicability.

> Note: A device running the application will only install the application using either Compose files or Helm Charts but not both.

## Application Description Examples

### Example 1: Simple Application Description

A simple hello-world example of an `ApplicationDescription` is shown below:

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

### Example 2: Application Description with Deployment Profiles for Helm and Compose

An example of an `ApplicationDescription` defining deployment profiles for both cases, Helm chart as well as Compose, is shown below:

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

## Example Workflow

The following workflow shows how a [workload fleet manager](#workload-fleet-manager) might use the application description information:

1. End User visits an [workload catalog](#workload-marketplace) of the [Workload Fleet Manager](#workload-fleet-manager) Frontend
2. Frontend requests all [workloads](#workload) from the [Workload Fleet Manager](#workload-fleet-manager)
3. Either: the [Workload Fleet Manager](#workload-fleet-manager) requests all application descriptions from each known [Application Registry](#application-registry), or maintains a cache of application descriptions
4. The [Workload Fleet Manager](#workload-fleet-manager) returns the retrieved documents of application descriptions to the frontend
5. The frontend parses the metadata element of all received application description documents
6. The frontend presents the parsed metadata in a UI to the end user
7. The end user selects the [workload](#workload) to be installed
8. The frontend parses the configuration element of the selected application description
9. The frontend presents the parsed configuration to the user
10. The end user fills out the configurable application parameters to be applied to the [workload](#workload)
11. The frontend creates an ApplicationDeployment definition and sends it to the [Workload Fleet Manager](#workload-fleet-manager)

## Application Registry Interaction

The Application Developer SHALL use a Git repository to share an [application package](#application-package). This Git repository is considered the [Application Registry](#application-registry).

The connectivity between the Workload Orchestration Software and the [Application Registry](#application-registry) SHALL be read-only.

Upon installation request from the End User, the Workload Orchestration Vendor SHALL retrieve the [application package](#application-package) using a git pull request from the [Application Registry](#application-registry).

At a minimum, a Margo-compliant WOS SHALL provide a way for an end user to manually setup a connection between the WOS and an [application registry](#application-registry).

### Secure Access to the Application Package

The connection between the Workload Orchestration software and the Application developer's [application registry](#application-registry) should be secured using standard secure connectivity best practices:
- Basic authentication via HTTPS
- Bearer token authentication
- TLS certifications

## Publishing Workload Observability Data

Compliant [workloads](#workload) MAY choose to expose workload specific observability data by sending their observability data to the OpenTelemetry collector on the standalone device or cluster. While this is optional, it is highly recommended in order to support distributed diagnostics.

**Requirements:**
- [Workload suppliers](#workload-supplier) choosing to expose metrics, traces or logs for consumption with OpenTelemetry MUST send the data to the OpenTelemetry collector using OTLP
- The information required to communicate with the device's OTEL Collector is injected into each container using environment variables
- [Workload suppliers](#workload-supplier) SHOULD NOT expect their [workloads](#workload) to be auto-instrumented by anything outside of their control
- A [workload supplier](#workload-supplier) MAY choose an observability framework other than OpenTelemetry but it MUST be self-contained within the deployment of their [workload](#workload)
- If an alternative approach is taken, it is NOT recommended [workload suppliers](#workload-supplier) publish their observability data outside the device/cluster by using any other means other than the OpenTelemetry collector
- If the [workload supplier](#workload-supplier) chooses to export data without using the OpenTelemetry collector, they MUST NOT do this without the end user's approval

## Local Registries

Local registries provide options for configuring the usage of local OCI-based container (or Helm Chart) registries. The goal is to avoid reliance on public, Internet-accessible registries for reasons including:
1. Publicly hosted container images or Helm charts could become unavailable
2. Internet connectivity may not be available to the device
3. End-users want to host their own registries for security scans and validation

### Device Categories by Connectivity

- **Fully connected device:** Device deployed in the field with Internet access
- **Locally connected device:** Device has connectivity to a local network and a local repository can be made reachable
- **Air-gapped device:** Device generally not connected and must be configured by accessing it directly

### Local Registry Configuration Options

**Option 1: Container Registry Mirror on Kubernetes Level**

Kubernetes supports the configuration of registry mirrors. For k3s, the file `/etc/rancher/k3s/registries.yaml` can be used:

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

Configure a Docker Registry as caching proxy using `config.yml`:

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

Workload Management is a critical functionality that enables deployment and maintenance of [workloads](#workload) that are deployed to the customer's edge to enable business goals. In order to achieve Margo's [interoperability](#terms-and-definitions) mission statement, the Margo Management Interface is a critical component that enables interoperability between [Workload Fleet Management](#workload-fleet-manager) Software vendors and Device Vendors.

The main goals of the management interface are:
- By hosting the server side of the interface, [Workload Fleet Managers](#workload-fleet-manager) are enabled with the ability to onboard and manage [workloads](#workload) on all Margo compliant devices
- Device Vendors are able to build devices that include the client side of the interface which enables workload management via all Margo compliant [fleet managers](#workload-fleet-manager)

## Workload Deployment Sequence

<img width="904" height="568" alt="image" src="https://github.com/user-attachments/assets/fe8670e5-2f5f-418a-9d23-b0cffc0efec6" />

The complete workload deployment sequence includes:

1. End User visits App Catalog Page
2. [Workload Fleet Manager](#workload-fleet-manager) Frontend gets list of available [workloads](#workload)
3. End User selects [workload](#workload) to install
4. Frontend gets list of devices and filters on capability match
5. End User selects Device(s) and answers configurable questions
6. [Workload Fleet Manager](#workload-fleet-manager) creates & stores new deployment specification(s)
7. [Workload Fleet Management Client](#workload-fleet-management-client) continuously checks for new deployment specifications
8. Client pulls deployment specification(s) and workload manifest
9. Container Orchestrator initiates workload installation components
10. Container Runtime pulls OCI Containers
11. [Workload Fleet Management Client](#workload-fleet-management-client) provides component status updates
12. Full deployment status is provided and UI is updated

## Management Interface Requirements

The Management Interface MUST provide the following functionality:
- Device onboarding with the workload orchestration solution
- Device capabilities reporting
- Identifying desired state changes
- Deployment status reporting

### Implementation Requirements

- [Workload Fleet Management](#workload-fleet-manager) vendors MUST implement the server side of the [API specification](#api-reference)
- Device vendors MUST implement a client following the [API specification](#api-reference)
- The solution MUST maintain a storage repository for desired state files (Margo does not dictate how the desired state files are stored, other than ensuring they are available upon request)
- The device's management client MUST retrieve the device's set of desired state files from the [Workload Fleet Manager](#workload-fleet-manager)
- Interface patterns MUST support extended device communication downtime
- The Management Interface MUST reference industry [security protocols](#security-requirements) and port assignments for both client and server interactions
- Running the device's management client as containerized services is preferred but not required

### Configuration Requirements

The Management Interface MUST allow end-user configuration of:
- **Downtime configuration:** Ensures the device's management client is not retrying communication during known downtime. Communication errors MUST be ignored during this configurable period.
- **Polling Interval Period:** Configurable time period indicating the hours in which the device's management client checks for updates to the device's desired state
- **Polling Interval Rate:** Rate for how frequently the device's management client checks for updates to the device's desired state

## Edge Device Onboarding

The onboarding process includes:

1. End user provides the [Workload Fleet Management](#workload-fleet-manager) web service's root URL to the device's management client
2. Device's management client downloads the workload orchestration solution vendor's public root CA certificate using the [Onboarding API](#onboarding-api)
3. Context and trust is established between the device's management client and the [Workload Fleet Management](#workload-fleet-manager) web service
4. Device's management client uses the [Onboarding API](#onboarding-api) to onboard with the workload orchestration service
5. Device's management client receives the client Id, client secret and token endpoint URL used to generate a bearer token
6. Device's management client receives the URL for the Git repository containing its desired state and an associated access token for authentication
7. Device capabilities information is sent from the device to the workload orchestration web service using the [Device API](#device-api)

<img width="558" height="942" alt="image" src="https://github.com/user-attachments/assets/f0eae282-7f87-46ff-b101-75ff8bfd5407" />

### Configuring the Workload Fleet Management Web Service URL

To ensure the management client is configured to communicate with the correct [Workload Fleet Management](#workload-fleet-manager) web service, the device's management client needs to be configured with the expected URL. The device vendor MUST provide a way for the end user to manually set the URL the device's management client uses to communicate with the workload orchestration solution chosen by the end user.

### Margo Web API Authentication Method

The Margo Web API communication pattern between the device's management client and the workload orchestration web service must use a secure communication channel. Margo requires the use of oAuth 2.0 for authentication.

**API Authorization Strategy:**
- During the onboarding process the [Workload Fleet Management's](#workload-fleet-manager) web service provides the management client with a client Id, client secret and token endpoint URL
- The management client uses this information to create a bearer token for each request
- The bearer token is set in the Authorization header for each web request sent to the [Workload Fleet Management's](#workload-fleet-manager) web service requiring authorization

### Payload Security Method

Because of limitations using mTLS with common OT infrastructure such as TLS terminating HTTPS load-balancer or a HTTPS proxy doing lawful inspection, Margo has adopted a certificate-based payload signing approach to protect payloads from being tampered with.

**Process:**
- During the onboarding process the end user uploads the device's x.509 certificate to the workload orchestration solution
- The device's management client downloads the root CA certificate using the [Onboarding API](#onboarding-api)
- Once complete, both parties are able to secure their payloads

**Message Envelope Details:**
Once the edge device has a message prepared for the [Workload Fleet Management's](#workload-fleet-manager) web service, it completes the following to secure the message:
1. The device's management client calculates a digest and signature of the payload
2. The device's management client adds an envelope around it that has:
   - Actual payload
   - SHA of the payload, signed by the device certificate
   - Identifier for the certificate (MUST be the GUID provided by the device manufacturer, typically the hardware serial number)
3. The envelope is sent as the payload to the [Workload Fleet Management's](#workload-fleet-manager) web service
4. The [Workload Fleet Management's](#workload-fleet-manager) web service treats the request's payload as envelope structure, and receives the certificate identifier
5. The [Workload Fleet Management's](#workload-fleet-manager) web service computes digest from the payload, and verifies the signature using the device certification
6. The payload is then processed by the [Workload Fleet Management's](#workload-fleet-manager) web service

### GitOps Service Enrollment
**Authorization methods for the Desired State Git Repository**
- Git access tokens shall be provided to the device's management client. These access tokens MUST be tied to a dedicated non-user account for access where credentials are frequently rotated and short lived.
- Need to support accessing rotations of tokens

## Device Capability Reporting

The purpose of device capabilities reporting is to ensure the [Workload Fleet Management](#workload-fleet-manager) solution has the information needed to pair [workloads](#workload) with compatible [edge devices](#edge-compute-device).

**Device Capability Reporting Requirements:**
- The device owner MUST report their device's capabilities and characteristics via the [Device API](#device-api) when onboarding the device with the workload orchestration solution
- During the lifecycle of the Edge device, if there is a change that impacts the reported characteristics, the device's interface shall send an update to the [Workload Fleet Manager](#workload-fleet-manager)

**Required Information:**
- Device Id
- Device Vendor
- Model Number
- Serial Number
- Margo Device Role Designation (Cluster Leader/Worker / Standalone Device)
- Resources available for [workloads](#workload):
  - Memory Capacity
  - Storage Capacity
  - CPU information
  - Device peripherals (i.e. Graphics card)
  - Network interfaces (wifi/eth/cellular)

## Workload Deployment

Margo uses an OpenGitOps approach for managing the [edge device's](#edge-compute-device) desired state. The workload orchestration solution vendor maintains Git repositories, under their control, to push updates to the desired state for each device being managed.

### Desired State Requirements

- The workload orchestration solution MUST store the device's desired state documents within a Git repository the device's management client can access
- The device's management client MUST monitor the device's Git repository for updates to the desired state using the URL and access token provided by the workload orchestration solution during onboarding

### Workload Management Sequence of Operations

**Desired State lifecycle:**
1. The workload orchestration solution creates the desired state documents based on the end user's inputs when installing, updating or deleting an application
2. The workload orchestration solution pushes updates to the device's Git repository reflecting the changes to the desired state
3. The device's management client monitors its assigned Git repository for changes
4. When the device's management client notices a difference between the current (running) state and the desired state, it MUST pull down and attempt to apply the new desired state

**Applying the Desired State:**
- The device attempts to apply the desired state to become new current state
- While the new desired state is being applied, the device's management client MUST report progress on state changes using the [Device API](#device-api)

### Deployment Status

The deployment status is sent to the workload orchestration web service using the [Device API](#device-api) when there is a change in the deployment state.

**Deployment status rules:**
- The state is `Pending` once the device management client has received the updated desired state but has not started applying it. When reporting this state indicate the reason (such as waiting on Policy agent or waiting on other applications)
- The state is `Installing` once the device management client has started the process of applying the desired state
- The state is `Failure` if at any point the desired state fails to be applied. When reporting a Failure state the error message and error code MUST be reported
- The state is `Success` once the desired state has been applied completely

## Consuming Workload Observability Data

[Workload Fleet Management](#workload-fleet-manager) or observability platform suppliers MAY choose to consume [workload observability](#workload-observability) data exported from the end user's devices to provide valuable services to the end user.

The end user MAY choose to export observability data from Margo compliant devices to other OpenTelemetry collectors or backends within their environment that is not on the device.

# Device Interoperability

## Edge Compute Devices Overview

Within Margo, devices are represented by compute hardware that host Margo compliant [workloads](#workload) within the customer's environment. Devices are defined by the roles they can provide the Margo ecosystem. Each role has its own requirements which enable unique functionality such as cluster management. These Margo devices ensure the [Workload Developers](#workload-supplier) with guaranteed functionality they can depend on being present in every compliant device. These devices are onboarded and managed by a single [Workload Fleet Manager](#workload-fleet-manager).

The promise the Margo specification provides Device vendors the following benefits:
- By producing a Margo compliant device it is compatible with ALL Margo compliant [Fleet Managers](#workload-fleet-manager)
- Device vendors have freedom of implementation regarding the specific components, i.e. oci container runtime, as long as the component provides the agreed upon functionality the Application Vendors expect.

### Supported Device roles are shown below:

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

Within Margo, devices are represented by compute hardware that runs within the customer's environment to enable the system with Margo Compliant [Workloads](#workload). An Edge Compute within Margo is initially referenced as a "Device" which represents the initial lifecycle stage. Once the device is onboarded within the Workload Orchestration Software, it assumes a role based on capabilities.

### Cluster Leader Role

The Cluster Leader Role within Margo describes devices that have the ability to manage the cluster of one or more worker nodes that are connected. Additionally, Cluster Leader can also host Margo compliant [workloads](#workload).

**Functional Requirements:**
- Host the [Workload Fleet Management Client](#workload-fleet-management-client) (enables functionality outlined within the [Workload Management](#fleet-management) section)
- Enable management functionality via Kubernetes API
- Host additional components:
  - Helm Client (Provider)
  - OCI Container Runtime
  - OTEL Collector
  - Policy Agent
- Host [Device Fleet Management Client](#device-fleet-management-client)

### Cluster Worker Role

The Cluster Worker Role within Margo describes devices that have a limited amount of compute capacity that can still run Margo compliant [workloads](#workload). This role is managed via the Cluster Leader.

**Functional Requirements:**
- Enable Worker Node functionality within a Kubernetes Cluster via the kubelet service
- Host additional components:
  - OCI Container Runtime
  - OTEL Collector
  - Policy Agent
- Host [Device Fleet Management Client](#device-fleet-management-client)

### Standalone Cluster Role

The Standalone Cluster Role within Margo describes devices that have additional compute capacity to enable a wide range of functions within the ecosystem.

**Functional Requirements:**
- Enables both Cluster Leader and Worker roles concurrently in a single device
- See [Cluster Leader Role](#cluster-leader-role) and [Cluster Worker Role](#cluster-worker-role) for specific requirements
- The Standalone Cluster role has the ability to transition into either the Cluster Leader or Cluster Worker at a later stage in its lifecycle deployment

### Standalone Device Role

The Standalone Device role represents a device that can host Margo Compliant [Workloads](#workload). This device role is not intended to be utilized within a clustered configuration and typically consists of devices within limited amount of resources to run [workloads](#workload).

**Functional Requirements:**
- Host the [Workload Fleet Management Client](#workload-fleet-management-client) (enables functionality outlined within the [Workload Management](#fleet-management) section)
- Host additional components:
  - OCI Container Runtime
  - OTEL Collector
  - Policy Agent
- Host [Device Fleet Management Client](#device-fleet-management-client)

## Collecting Workload Observability Data

The device owner MUST deploy, and configure, an OpenTelemetry collector on their device. The device owner MAY choose the deployment model they wish to follow but MUST use one of the following approaches.

### OpenTelemetry Collector Deployment Requirements

- For standalone and clustered devices there MUST be at least one OpenTelemetry collector deployed to collect the observability data required
- The Device owner MAY choose to deploy multiple OpenTelemetry collectors with each collector receiving different parts of the observability data as long as all required observability data is collected.

<img width="627" height="291" alt="image" src="https://github.com/user-attachments/assets/03ab6569-1fe4-4ffd-9a9d-b2182867e600" />

- For multi-node capable clusters the device owner MAY chose to use the DaemonSet deployment model to ensure there is an OpenTelemetry collector running on each node

<img width="668" height="299" alt="image" src="https://github.com/user-attachments/assets/0b10967b-399a-44b8-8652-fa876bfe16c8" />

- For multi-node capable clusters the device owner MUST ensure the communication between [workloads](#workload), and collector, from one node to a collector on a different node is secure
- The device owner MUST NOT require the use of the sidecar deployment model at this time
- The device owner MUST NOT pre-configure exporters to send observability data from the device because the end user must control what observability data is exported
- The device owner MUST NOT attempt to inject auto-instrumentation into any compliant [workloads](#workload) running on the device that are not owned by the device owner

### Container Platform Observability Requirements

**Kubernetes Requirements:**
For devices running Kubernetes the following is a minimum list of observability data that MUST be provided:

- **Cluster observability data MUST be collected**
  - It is recommended the Device Owner use the Kubernetes Cluster Receiver with the default configuration
  - If not using the Kubernetes Cluster Receiver they MUST provide the same output as the default configuration
- **Cluster events observability data MUST be collected**
  - It is recommended to use either the Kubernetes Objects Receiver or Kubernetes Events Receiver with default configuration
  - If not using these receivers they MUST provide the same output as the default configuration
- **Node, Pod and Container observability data MUST be collected**
  - It is recommended to use the Kubelet Stats Receiver with default configuration
  - If not using this receiver they MUST provide the same output as the default configuration
- **Metadata identifying the observability data's source MUST be added**
  - It is recommended to use the Kubernetes Attributes Processor with default configuration
  - If not using this processor they MUST provide the same metadata as the default configuration

**Standalone Device Container Platforms:**
For devices running non-clustered container platforms such as Docker or Podman:

- **Container observability data MUST be collected**
  - It is recommended to use the Docker Stats Receiver or Podman Stats Receiver with default configuration
  - If not using these receivers they MUST provide the same output as the default configuration

**General Requirements:**
- The collector MUST receive data using the OLTP format
- It is recommended to use the OLTP Receiver to allow [workloads](#workload) to send observability data to the collector
- **Host observability data MUST be collected**
  - It is recommended to use the Host Metrics Receiver with default configuration
  - If not using this receiver they MUST provide the same output as the default configuration

### Workload Fleet Management Client Observability Requirements

For several reasons, it is recommended the [Workload Fleet Management Client](#workload-fleet-management-client) be deployed as a containerized [workload](#workload). If it is deployed this way, the [workload's](#workload) resource utilization observability data is captured automatically as part of the container platform observability requirements.

If the device owner chooses not to deploy the [Workload Fleet Management Client](#workload-fleet-management-client) as a containerized [workload](#workload) they MUST ensure resource usage observability data is available from the OpenTelemetry collector for their client.

### Connecting to the OpenTelemetry Collector

In order for a [workload](#workload) to publish its observability data to the collector on the standalone device or cluster the device owner MUST inject the following environment variables into each container:

| Environment Variable | Description |
|---------------------|-------------|
| GRPC_OTEL_EXPORTER_OTLP_ENDPOINT | (Optional) The URL for the [workload](#workload) to use to connect to the OpenTelemetry collector using gRPC |
| HTTP_OTEL_EXPORTER_OTLP_ENDPOINT | (Required) The URL for the [workload](#workload) to use to connect to the OpenTelemetry collector HTTP + protobuf |
| OTEL_EXPORTER_OTLP_CERTIFICATE | (Optional)The PATH for the client certificate (in PEM format) to use for secure connections to the OpenTelemetry Collector. The workload must connect using the certificate if it is provided.|
| OTEL_EXPORTER_OTLP_PROTOCOL | (Optional) "grpc" if the preferred protocol is gRPC, "http/protobuf" if the preferred protocol is HTTP + protobuf. The default is "http/protobuf" if nothing is provided for this environment variable. If the preferred protocol is "grpc" but no gRPC endpoint is provided, or if the workload client cannot connect via gRPC, the workload client connects using "http/protobuf". |

### Exporting Observability Data

End users MUST be able to export observability data from a standalone device or cluster to collectors, or backends, onsite or in the cloud if they wish to make the information available to enable remote monitoring and diagnostics.

OpenTelemetry allows using either a push or pull approach for getting data from a collector. Cloud based [workload fleet management](#workload-fleet-manager) or observability platform service vendors should NOT require a pull method for collecting observability data because most end users will not allow devices to be exposed to the internet because of security concerns.

### Workload Observability Default Telemetry

#### Metrics
The following telemetry data is collected by using the default configurations for the receivers indicated above. You can find more information about each piece of telemetry from the receiver's documentation.

Here's the markdown table based on the provided text:

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
The Kubernetes Events receiver collects the event logs when using the default configuration. The Kubernetes Object Receiver must be configured to collect the desired logs. Container logs must be emitted using OTLP.

#### Kubernetes Attributes Processor

The following shows the attributes added to each signal when using the Kubernetes Attribute Processors' default configuration.

- k8s.namespace.name
- k8s.pod.name
- k8s.pod.uid
- k8s.pod.start_time
- k8s.deployment.name
- k8s.node.name

# Workload Observability

## Overview

Observability involves the collection and analysis of information produced by a system to monitor its internal behavior.

Observability data is captured using the following signals:
- **Metrics:** A numerical measurement in time used to observe change over a period of time or configured limits. For example, memory consumption, CPU Usage, available disk space.
- **Logs:** Text outputs produced by a running system/[workloads](#workload) to provide information about what is happening. For example, outputs to capture security events such as failed login attempts, or unexpected conditions such as errors.
- **Traces:** Contextual data used to follow a request's entire path through a distributed system. For example, trace data can be used to identify bottlenecks, or failure points, within a distributed system.

## Margo's Workload Observability Scope

Margo's [workload observability](#workload-observability) scope is limited to the following areas:
- The device's container platform
- The device's [Workload Fleet Management Client](#workload-fleet-management-client)
- The compliant [workloads](#workload) deployed to the device

## Intended Use Cases

The [workload observability](#workload-observability) data is intended to be used for purposes such as:
- Monitoring the container platform's health and current state. This includes aspects such as memory, CPU, and disk usage as well as cluster, node, pod, and container availability, run state, and configured resource limits. This enables end users to make decisions such as whether or not a device can support more [workloads](#workload), or has too many deployed.
- Monitoring the [Workload Fleet Management Client](#workload-fleet-management-client) and containerized [workload's](#workload) state to ensure it is running correctly, performing as expected and not consuming more resource than expected.
- To assist with debugging/diagnostics for [workloads](#workload) encountering unexpected conditions impacting their ability to run as expected.

**Important:** Margo's [workload observability](#workload-observability) is NOT intended to be used to monitor anything outside the device such as production processes, machinery, controllers, or sensors and should NOT be used for this purpose.

## Workload Observability Framework

[Workload observability](#workload-observability) data is made available using OpenTelemetry. OpenTelemetry, a popular open source specification, defines a common way for observability data to be generated and consumed.

**Reasons for choosing OpenTelemetry:**
- OpenTelemetry is based on an open source specification and not only an implementation
- OpenTelemetry is widely adopted
- OpenTelemetry has a large, and active, open source community
- OpenTelemetry provides SDKs for many popular languages if people wish to use them
- The OpenTelemetry community project has reusable components such as telemetry receivers for Kubernetes, Docker and the host system making integration easier
- OpenTelemetry is vendor agnostic

## Enhanced OpenTelemetry Configuration

### OpenTelemetry Collector Configuration Example

Below is a comprehensive example of an OpenTelemetry collector configuration that aligns with Margo requirements:

```yaml
receivers:
  otlp:
    protocols:
      grpc:
        endpoint: 0.0.0.0:4317
      http:
        endpoint: 0.0.0.0:4318
  hostmetrics:
    collection_interval: 10s
    scrapers:
      cpu:
      disk:
      filesystem:
      memory:
      network:
      process:
  k8s_cluster:
    auth_type: serviceAccount
    node_conditions_to_report: [Ready, MemoryPressure, DiskPressure, PIDPressure]
    allocatable_types_to_report: [cpu, memory, ephemeral-storage, storage]
  kubeletstats:
    collection_interval: 20s
    auth_type: serviceAccount
    endpoint: https://${env:K8S_NODE_NAME}:10250
    insecure_skip_verify: true
    metric_groups:
      - node
      - pod
      - container
      - volume
  k8s_events:
    auth_type: serviceAccount
  docker_stats:
    endpoint: unix:///var/run/docker.sock
    collection_interval: 10s
    timeout: 5s

processors:
  k8sattributes:
    auth_type: serviceAccount
    passthrough: false
    filter:
      node_from_env_var: KUBE_NODE_NAME
    extract:
      metadata:
        - k8s.pod.name
        - k8s.pod.uid
        - k8s.deployment.name
        - k8s.namespace.name
        - k8s.node.name
        - k8s.pod.start_time
    pod_association:
      - sources:
        - from: resource_attribute
          name: k8s.pod.ip
      - sources:
        - from: resource_attribute
          name: k8s.pod.uid
      - sources:
        - from: connection
  batch:
    timeout: 1s
    send_batch_size: 1024
    send_batch_max_size: 2048
  memory_limiter:
    limit_mib: 512

exporters:
  logging:
    loglevel: info
  # Additional exporters can be configured here
  # Example: prometheus, jaeger, zipkin, etc.
  # prometheus:
  #   endpoint: "0.0.0.0:8889"
  # jaeger:
  #   endpoint: jaeger-collector:14250
  #   tls:
  #     insecure: true

service:
  pipelines:
    metrics:
      receivers: [otlp, hostmetrics, k8s_cluster, kubeletstats, docker_stats]
      processors: [k8sattributes, memory_limiter, batch]
      exporters: [logging]
    traces:
      receivers: [otlp]
      processors: [k8sattributes, memory_limiter, batch]
      exporters: [logging]
    logs:
      receivers: [otlp, k8s_events]
      processors: [k8sattributes, memory_limiter, batch]
      exporters: [logging]
  extensions: [health_check, pprof, zpages]
```

# API Reference

## Margo Management API Specification

The Margo Management API is used to enable communication between Margo compliant devices and orchestration solutions.

## Authorization and Security

### Authorization Header

For requests requiring authentication, a bearer token MUST be present in the message's Authorization header.

You can get the access token by sending a request to the workload orchestration web service's token URL, providing the device's client Id and secret.

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

Set the Authorization header's value to `Bearer <ACCESS_TOKEN>` when making requests requiring authorization.

### Payload Signing

**Message Envelope Structure:**

```json
{
  "payload": "<actual-payload-content>",
  "signature": "<base64-encoded-signature>",
  "certificateId": "<device-certificate-guid>"
}
```

**Signing Process:**

1. Calculate SHA-256 hash of the payload
2. Sign the hash using the device's private key
3. Base-64 encode the signature
4. Include the certificate identifier (device GUID/serial number)
5. Send the envelope as the request payload

**Verification Process:**

1. Extract the certificate identifier from the envelope
2. Retrieve the corresponding public key
3. Verify the signature using the public key
4. Compare the hash of the received payload with the decrypted signature
5. Process the payload if verification succeeds

### Verifying Signed Payloads

Steps for verifying signed payloads:
1. Retrieve the public key from the `X-Payload-Signature` header
2. Decrypt the digital signature using the public key to get the original hash value
3. Generate a SHA-256 hash value for the request's body
4. Ensure the generated hash value matches the hash value from the message

### Desired State API

The desired state is expressed as a Kubernetes custom resource definition and made available to the device's management client as a YAML document using the OpenGitOps pattern.

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
| apiVersion | string | Y | Identifier of the version of the API the object definition follows |
| kind | string | Y | Must be ApplicationDeployment |
| metadata | Metadata | Y | Metadata element specifying characteristics about the application deployment |
| spec | Spec | Y | Spec element that defines deployment profile and parameters associated with the application deployment |

**Metadata Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| annotations | Annotations | Y | Defines the application ID and unique identifier associated to the deployment specification |
| name | string | Y | When deploying to Kubernetes, the manifest's name |
| namespace | string | Y | When deploying to Kubernetes, the namespace the manifest is added under |

**Annotations Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| applicationId | string | Y | An identifier for the application. Must be lower case letters and numbers and MAY contain dashes. MUST NOT be more than 200 characters |
| id | string | Y | The unique identifier UUID of the deployment specification. Needs to be assigned by the Workload Orchestration Software |

## Application Package API - Application Description

The application description has the purpose of presenting the application, e.g., on an [application catalog](#application-catalog) or [marketplace](#workload-marketplace) from where an end user selects an application to be installed.

**Top-level Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| apiVersion | string | Y | Identifier of the version of the API the object definition follows |
| kind | string | Y | Specifies the object type; must be 'application' |
| metadata | Metadata | Y | Metadata element specifying characteristics about the application deployment |
| deploymentProfiles | []DeploymentProfile | Y | Deployment profiles for the application |
| parameters | map[string]Parameter | N | Configurable parameters for the application |
| configuration | Configuration | N | Configuration layout and validation rules |

**Metadata Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| id | string | Y | An identifier for the application. Must be lower case letters and numbers and MAY contain dashes. MUST NOT be more than 200 characters |
| name | string | Y | The application's official name for display purposes |
| description | string | N | Description of the application |
| version | string | Y | The application's version |
| catalog | Catalog | Y | Catalog element specifying the application catalog details |

**DeploymentProfile Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| type | string | Y | Defines the type of deployment configuration. Allowed values are helm.v3 and compose |
| components | []Component | Y | Component element indicating the components to deploy when installing the application |

**Component Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| name | string | Y | A unique name used to identify the component package. Must be lower case letters and numbers and MAY contain dashes |
| properties | ComponentProperties | Y | A dictionary element specifying the component package's deployment details |

**ComponentProperties for helm.v3:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| repository | string | Y | The URL indicating the helm chart's location |
| revision | string | Y | The helm chart's full version |
| wait | bool | N | If True, indicates the device MUST wait until the helm chart has finished installing before installing the next helm chart |
| timeout | string | N | The time to wait for the component's installation to complete. Format is "##m##s" |

**ComponentProperties for compose:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| packageLocation | string | Y | The URL indicating the Compose package's location |
| keyLocation | string | N | The public key used to validate the digitally signed package. When signing the package PGP MUST be used |
| wait | bool | N | If True, indicates the device MUST wait until the Compose file has finished starting up |
| timeout | string | N | The time to wait for the component's installation to complete |

**Parameter Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| name | string | Y | Name of the parameter |
| value | <see description> | N | The parameter's default value. Accepted data types are string, integer, double, boolean, array[string], array[integer], array[double], array[boolean] |
| targets | []Target | Y | Used to indicate which component the value should be applied to when installing, or updating, the application |

**Target Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| pointer | string | Y | The name of the parameter in the deployment configuration. For Helm deployments, this is the dot notation for the matching element in the values.yaml file. For compose deployments, this is the name of the environment variable to set |
| components | []string | Y | Indicates which deployment profile component the parameter target applies to |

**Configuration Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| sections | []Section | Y | Sections are used to group related parameters together |
| schema | []Schema | Y | Schema definitions for parameter validation |

**Section Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| name | string | Y | The name of the section |
| settings | []Setting | Y | Settings are used to provide instructions for displaying parameters to the user |

**Setting Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| parameter | string | Y | The name of the parameter this setting applies to |
| name | string | Y | Display name for the parameter |
| description | string | N | Description of the parameter |
| schema | string | Y | Reference to the schema definition for validation |
| immutable | bool | N | If true, indicates the parameter cannot be changed after initial deployment |

**Schema Attributes:**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| name | string | Y | The name of the schema rule |
| dataType | string | Y | The data type. Valid values: string, integer, double, boolean, array |
| allowEmpty | boolean | N | If true, indicates a value must be provided. Default is false |

**TextValidationSchema Attributes (extends Schema):**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| minLength | integer | N | Minimum number of characters required |
| maxLength | integer | N | Maximum number of characters allowed |
| regexMatch | string | N | Regular expression pattern for validation |

**NumericIntegerValidationSchema Attributes (extends Schema):**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| minValue | integer | N | Minimum allowed integer value |
| maxValue | integer | N | Maximum allowed integer value |

**NumericDoubleValidationSchema Attributes (extends Schema):**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| minValue | float | N | Minimum allowed value |
| maxValue | float | N | Maximum allowed value |
| minPrecision | integer | N | Minimum level of precision required |
| maxPrecision | integer | N | Maximum level of precision allowed |

**BooleanValidationSchema Attributes (extends Schema):**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| allowEmpty | boolean | N | If true, indicates a value must be provided |

**SelectValidationSchema Attributes (extends Schema):**

| Attribute | Type | Required? | Description |
|-----------|------|-----------|-------------|
| multiselect | boolean | N | If true, indicates multiple values can be selected |
| options | []string | Y | List of acceptable options the user can select from |

## Device API

### Device Capabilities

Devices MUST provide the workload orchestration service with its capabilities and characteristics using the Device API's device capabilities endpoint.

**Route and HTTP Methods:**
- `POST /device/{deviceId}/capabilities`
- `PUT /device/{deviceId}/capabilities`

**Route Parameters:**

| Parameter | Type | Required? | Description |
|-----------|------|-----------|-------------|
| {deviceId} | string | Y | The device's Id registered with the workload orchestration web service during onboarding |

**Request Body Fields:**

| Field | Type | Required? | Description |
|-------|------|-----------|-------------|
| apiVersion | string | Y | Identifier of the version of the API the object definition follows |
| kind | string | Y | Must be DeviceCapabilities |
| properties | Properties | Y | Element that defines characteristics about the device |

**Properties Fields:**

| Field | Type | Required? | Description |
|-------|------|-----------|-------------|
| id | string | Y | Unique deviceID assigned to the device via the Device Owner |
| vendor | string | Y | Defines the device vendor |
| modelNumber | string | Y | Defines the model number of the device |
| serialNumber | string | Y | Defines the serial number of the device |
| roles | []string | Y | Element that defines the device role it can provide. MUST be one of: Standalone Cluster, Cluster Leader, or Standalone Device |
| resources | Resources | Y | Element that defines the device's resources available to applications |
| peripherals | []Peripheral | Y | Element that defines the device's peripherals available to applications |
| interfaces | []Interface | Y | Element that defines the device's interfaces available to applications |

**Resources Fields:**

| Field | Type | Required? | Description |
|-------|------|-----------|-------------|
| memory | string | Y | Total available memory (e.g., "64.0 GB") |
| storage | string | Y | Total available storage (e.g., "2000 GB") |
| cpus | []CPU | Y | Array of CPU specifications |

**CPU Fields:**

| Field | Type | Required? | Description |
|-------|------|-----------|-------------|
| architecture | string | Y | CPU architecture (e.g., "Intel x64", "ARM64") |
| cores | integer | Y | Number of CPU cores |
| frequency | string | Y | CPU frequency (e.g., "6.2 GHz") |

**Peripheral Fields:**

| Field | Type | Required? | Description |
|-------|------|-----------|-------------|
| name | string | Y | Name of the peripheral |
| type | string | Y | Type of peripheral (e.g., "GPU", "USB", "Serial") |
| modelNumber | string | Y | Model number of the peripheral |
| properties | map[string]string | Y | Additional properties specific to the peripheral |

**Interface Fields:**

| Field | Type | Required? | Description |
|-------|------|-----------|-------------|
| name | string | Y | Name of the interface |
| type | string | Y | Type of interface (e.g., "Ethernet", "WiFi", "Cellular") |
| modelNumber | string | Y | Model number of the interface |
| properties | map[string]string | Y | Additional properties specific to the interface |

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

While applying a new desired state the device's management client MUST provide the workload orchestration web service with an indication of the current status using the Device API's device status endpoint.

**Route and HTTP Methods:**
- `POST /device/{deviceId}/deployment/{deploymentId}/status`

**Route Parameters:**

| Parameter | Type | Required? | Description |
|-----------|------|-----------|-------------|
| {deviceId} | string | Y | The device's Id registered with the workload orchestration solution during onboarding |
| {deploymentId} | string | Y | The deployment Id the status is being reported for |

**Request Body Fields:**

| Field | Type | Required? | Description |
|-------|------|-----------|-------------|
| apiVersion | string | Y | Identifier of the version of the API the object definition follows |
| kind | string | Y | Must be DeploymentStatus |
| deploymentId | string | Y | The unique identifier UUID of the deployment specification |
| status | Status | Y | Element that defines overall deployment status |
| components | []Component | Y | Element that defines the individual component's deployment status |

**Status Fields:**

| Field | Type | Required? | Description |
|-------|------|-----------|-------------|
| state | string | Y | Current state of the overall deployment. MUST be one of: Pending, Installing, Installed, Failed |
| error | Error | N | Element that defines the overall installation error if one occurred |

**Component Fields:**

| Field | Type | Required? | Description |
|-------|------|-----------|-------------|
| name | string | Y | Name of the component |
| state | string | Y | Current state of the component. MUST be one of: Pending, Installing, Installed, Failed |
| error | Error | N | Element that defines the component installation error if one occurred |

**Error Fields:**

| Field | Type | Required? | Description |
|-------|------|-----------|-------------|
| code | string | N | Error code identifying the type of error |
| message | string | N | Human-readable error message |

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
        "code": "",
        "message": ""
      }
    }
  ]
}
```

**Deployment Status State Definitions:**

- **Pending**: Device management client has received the updated desired state but has not started applying it. When reporting this state, indicate the reason such as:
  - Waiting on Policy agent
  - Waiting on other applications in the 'Order of operations' to be completed
  - Resource constraints preventing deployment

- **Installing**: Device management client has started the process of applying the desired state

- **Installed**: The desired state has been applied completely and the workload is running successfully

- **Failed**: At any point the desired state failed to be applied. When reporting a Failure state, the error message and error code MUST be reported

**Common Error Codes:**

- `AUTHENTICATION_FAILED`: OAuth or certificate authentication failed
- `DEPLOYMENT_FAILED`: Application deployment failed
- `RESOURCE_INSUFFICIENT`: Insufficient resources on device
- `NETWORK_ERROR`: Network connectivity issues
- `VALIDATION_ERROR`: Invalid configuration or parameters
- `TIMEOUT_ERROR`: Operation timed out
- `IMAGE_PULL_FAILED`: Failed to pull container image
- `CHART_NOT_FOUND`: Helm chart not found
- `COMPOSE_FILE_INVALID`: Invalid Compose file format

## Onboarding API

### Device Onboarding

**Route and HTTP Methods:**
- `POST /onboarding/device`

**Request Body Fields:**

| Field | Type | Required? | Description |
|-------|------|-----------|-------------|
| apiVersion | string | Y | Identifier of the version of the API the object definition follows |
| kind | string | Y | Must be DeviceOnboarding |
| deviceId | string | Y | Unique device identifier |
| certificate | string | Y | Base-64 encoded device certificate |
| capabilities | DeviceCapabilities | Y | Device capabilities as defined in Device Capabilities API |

**Response Body Fields:**

| Field | Type | Required? | Description |
|-------|------|-----------|-------------|
| clientId | string | Y | OAuth 2.0 client ID for authentication |
| clientSecret | string | Y | OAuth 2.0 client secret for authentication |
| tokenEndpoint | string | Y | URL for OAuth 2.0 token endpoint |
| gitRepositoryUrl | string | Y | URL for the Git repository containing desired state |
| gitAccessToken | string | Y | Access token for Git repository authentication |

**Example Request:**

```json
{
  "apiVersion": "onboarding.margo/v1",
  "kind": "DeviceOnboarding",
  "deviceId": "northstarida.xtapro.k8s.edge",
  "certificate": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0t...",
  "capabilities": {
    "apiVersion": "device.margo/v1",
    "kind": "DeviceCapability",
    "properties": {
      "id": "northstarida.xtapro.k8s.edge",
      "vendor": "Northstar Industrial Applications",
      "modelNumber": "332ANZE1-N1",
      "serialNumber": "PF45343-AA",
      "roles": ["standalone cluster"]
    }
  }
}
```

**Example Response:**

```json
{
  "clientId": "device-client-12345",
  "clientSecret": "super-secret-client-secret",
  "tokenEndpoint": "https://fleet-manager.example.com/oauth/token",
  "gitRepositoryUrl": "https://git.example.com/device-configs/northstarida.xtapro.k8s.edge",
  "gitAccessToken": "ghp_1234567890abcdef"
}
```

### Certificate API (Root CA Download)

In order to facilitate secure communication between the device's management client and workload orchestration web service the workload orchestration web service's root CA certificate must be downloaded using the Onboarding API's certificate endpoint.

**Route and HTTP Methods:**
- `GET /onboarding/certificate`

**Response Body:**

```json
{
  "certificate": "<base-64 encoded certificate text>"
}
```

## GitOps Repository Structure

**Recommended folder structure for device desired state:**

```
/
├── devices/
│   ├── {device-id}/
│   │   ├── applications/
│   │   │   ├── app1-deployment.yaml
│   │   │   ├── app2-deployment.yaml
│   │   └── device-config.yaml
│   └── {another-device-id}/
│       ├── applications/
│       └── device-config.yaml
└── shared/
    ├── policies/
    └── templates/
```

**Example ApplicationDeployment YAML:**

```yaml
apiVersion: application.margo.org/v1alpha1
kind: ApplicationDeployment
metadata:
  annotations:
    id: "a3e2f5dc-912e-494f-8395-52cf3769bc06"
    applicationId: "com-northstartida-digitron-orchestrator"
  name: digitron-orchestrator
  namespace: default
spec:
  deploymentProfile:
    type: helm.v3
    components:
      - name: digitron-orchestrator
        properties:
          repository: oci://northstarida.azurecr.io/charts/northstarida-digitron-orchestrator
          revision: 1.0.9
          wait: true
  parameters:
    idpName:
      value: "Active Directory"
      targets:
        - pointer: idp.name
          components: ["digitron-orchestrator"]
    pollFrequency:
      value: 30
      targets:
        - pointer: settings.pollFrequency
          components: ["digitron-orchestrator"]
```

# Security Requirements

## Authentication and Authorization

All API communications MUST use bearer token authentication with proper certificate-based payload signing.

### OAuth 2.0 Implementation

**Token Request Example:**

```bash
curl -X POST \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials&client_id=device-client-12345&client_secret=super-secret-client-secret" \
  https://fleet-manager.example.com/oauth/token
```

**Token Response:**

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "Bearer",
  "expires_in": 3600,
  "scope": "device:read device:write deployment:read deployment:write"
}
```

### Certificate-Based Payload Signing

**Message Envelope Structure:**

```json
{
  "payload": {
    "apiVersion": "device.margo/v1",
    "kind": "DeviceCapability",
    "properties": {
      "id": "device-123",
      "vendor": "Example Corp"
    }
  },
  "signature": "MEUCIQDxxx...",
  "certificateId": "device-123-serial-number"
}
```

**Payload Signing Process:**

1. **Generate Hash**: Calculate SHA-256 hash of the JSON payload
2. **Sign Hash**: Use device's private key to sign the hash
3. **Encode Signature**: Base64 encode the signature
4. **Create Envelope**: Wrap payload with signature and certificate ID

**Verification Process:**

1. **Extract Components**: Get payload, signature, and certificate ID from envelope
2. **Retrieve Certificate**: Get public key using certificate ID
3. **Verify Signature**: Decrypt signature using public key
4. **Compare Hashes**: Verify payload hash matches decrypted signature

## Device Security

Devices MUST implement:
- **TPM-based hardware security**: Trusted Platform Module for secure key storage
- **Secure boot processes**: Verified boot chain with signed bootloaders
- **Attestation mechanisms**: Hardware-based device identity verification
- **Zero Trust Network Access (ZTNA)**: Never trust, always verify approach

### TPM Requirements

- **TPM 2.0 compliance**: All devices must include TPM 2.0 or later
- **Key storage**: Private keys must be stored in TPM secure storage
- **Attestation**: Device identity must be verifiable through TPM attestation
- **Secure boot**: TPM must be used to verify boot integrity

### Secure Boot Implementation

```yaml
# Example secure boot configuration
secure_boot:
  enabled: true
  bootloader:
    signed: true
    verification_key: "/secure/bootloader.pub"
  kernel:
    signed: true
    verification_key: "/secure/kernel.pub"
  modules:
    signature_verification: true
```

## Communication Security

All communications between components MUST use:
- **TLS 1.3 encryption**: Modern TLS for all network communications
- **Certificate-based authentication**: Mutual TLS authentication
- **Payload signing and verification**: Digital signatures for message integrity
- **Industry-standard security protocols**: HTTPS, OAuth 2.0, X.509 certificates

### TLS Configuration Requirements

**Minimum TLS Requirements:**

- TLS 1.3 for all new connections
- TLS 1.2 acceptable for legacy support (with approved cipher suites)
- Certificate validation must be enforced
- Perfect Forward Secrecy (PFS) must be enabled

**Approved Cipher Suites for TLS 1.3:**

- TLS_AES_256_GCM_SHA384
- TLS_CHACHA20_POLY1305_SHA256
- TLS_AES_128_GCM_SHA256

**Certificate Requirements:**

- X.509 certificates with minimum 2048-bit RSA or 256-bit ECC
- Certificate chain validation required
- Certificate revocation checking (CRL or OCSP)
- Certificates must be issued by approved Certificate Authorities

### Network Security

**Firewall Configuration:**

```yaml
# Example firewall rules for Margo devices
firewall_rules:
  inbound:
    - port: 4317
      protocol: TCP
      source: workload_subnet
      description: "OpenTelemetry gRPC"
    - port: 4318
      protocol: TCP
      source: workload_subnet
      description: "OpenTelemetry HTTP"
    - port: 443
      protocol: TCP
      source: fleet_manager_subnet
      description: "Fleet Manager API"
  outbound:
    - port: 443
      protocol: TCP
      destination: fleet_manager
      description: "Fleet Manager Communication"
    - port: 53
      protocol: UDP
      destination: dns_servers
      description: "DNS Resolution"
```

**Zero Trust Network Access:**

- No implicit trust based on network location
- All communications must be authenticated and authorized
- Continuous monitoring and verification of device identity
- Micro-segmentation to limit lateral movement

# Compliance and Conformance

## Compliance Requirements

To be Margo-compliant, implementations MUST:
- Support all mandatory [API endpoints](#api-reference)
- Implement required [security mechanisms](#security-requirements)
- Pass compliance test suite validation
- Support specified application packaging formats

### Mandatory API Endpoints

**Device Management APIs:**
- `POST /device/{deviceId}/capabilities` - Device capability reporting
- `PUT /device/{deviceId}/capabilities` - Device capability updates
- `POST /device/{deviceId}/deployment/{deploymentId}/status` - Deployment status reporting

**Onboarding APIs:**
- `GET /onboarding/certificate` - Root CA certificate download
- `POST /onboarding/device` - Device onboarding

**Authentication APIs:**
- `POST /oauth/token` - OAuth 2.0 token endpoint

### Security Implementation Requirements

**Authentication:**
- OAuth 2.0 client credentials flow
- Bearer token authentication
- Certificate-based payload signing

**Encryption:**
- TLS 1.3 for all communications
- Certificate validation enforcement
- Perfect Forward Secrecy (PFS)

**Device Security:**
- TPM 2.0 support
- Secure boot implementation
- Hardware attestation capabilities

## Conformance Testing

The Margo compliance test suite validates:
- API compatibility and correctness
- Security implementation requirements
- Application package handling
- Device onboarding processes
- Observability data collection

### Test Categories

**Functional Testing:**
- API endpoint availability and response formats
- Authentication and authorization flows
- Application deployment workflows
- Device capability reporting

**Security Testing:**
- Certificate validation and trust establishment
- Payload signing and verification
- TLS configuration and cipher suite support
- OAuth 2.0 flow implementation

**Interoperability Testing:**
- Cross-vendor device and fleet manager compatibility
- Application package format validation
- OpenTelemetry data collection and export
- GitOps repository interaction

**Performance Testing:**
- API response times under load
- Memory and CPU usage during operations
- Network bandwidth utilization
- Concurrent deployment handling

### Compliance Certification Process

1. **Self-Assessment**: Vendors complete compliance checklist
2. **Automated Testing**: Run official compliance test suite
3. **Manual Review**: Expert review of complex scenarios
4. **Certification**: Issue compliance certificate upon successful completion
5. **Ongoing Monitoring**: Regular re-certification and updates

## Compliance Checklist

### Device Vendors

**Hardware Requirements:**
- [ ] TPM 2.0 support implemented
- [ ] Secure boot capability enabled
- [ ] Hardware attestation support
- [ ] Required connectivity interfaces

**Software Requirements:**
- [ ] Margo-compliant management client installed
- [ ] OpenTelemetry collector deployed and configured
- [ ] Container runtime (Docker, Podman, or Kubernetes)
- [ ] Policy agent implementation

**API Implementation:**
- [ ] Device capabilities API implemented
- [ ] Deployment status API implemented
- [ ] Onboarding API client implemented
- [ ] Certificate-based payload signing

### Fleet Manager Vendors

**API Server Requirements:**
- [ ] All mandatory API endpoints implemented
- [ ] OAuth 2.0 authentication server
- [ ] Certificate management and validation
- [ ] GitOps repository management

**Application Management:**
- [ ] Application registry integration
- [ ] Helm chart deployment support
- [ ] Docker Compose deployment support
- [ ] Parameter validation and injection

**Device Management:**
- [ ] Device onboarding workflow
- [ ] Capability-based device selection
- [ ] Deployment status monitoring
- [ ] Observability data collection

### Workload Suppliers

**Application Packaging:**
- [ ] Application description YAML format
- [ ] Helm chart packaging (for Kubernetes targets)
- [ ] Docker Compose packaging (for standalone targets)
- [ ] Digital signature implementation

**Configuration Management:**
- [ ] Parameter definition and validation
- [ ] Multi-deployment profile support
- [ ] Schema validation implementation
- [ ] Target component mapping

**Observability:**
- [ ] OpenTelemetry integration (optional but recommended)
- [ ] OTLP data export configuration
- [ ] Metrics, logs, and traces implementation
- [ ] Environment variable consumption

# Implementation Guidelines

## Contributing to Margo

### General Requirements

Contributions to Margo are typically very welcome! However, when adding new enhancements, maintainers must make a trade-off between the added value and the added cost of maintenance. It is recommended to first create a new issue on Github before starting the actual implementation and wait for feedback from the maintainers.

### Bug Fixes

Bug and security fixes are always welcome and take the highest priority.

### Contribution Checklist

- Contributions to the Specification must be covered by a Corporate CLA or Individual CLA
- Any code changes must be accompanied with automated tests
- Add the required copyright header to each new file introduced if appropriate
- Add `signed-off` to all commits to certify the "Developer's Certificate of Origin"
- Structure your commits logically, in small steps
- Base commits on top of latest `pre-draft` branch

### Signing the CLA

If you have not yet signed the Individual CLA, or your organization has not yet signed the Corporate CLA, the LFX EasyCLA bot will prompt you to follow the appropriate steps to authorize your contribution.

### Developer's Certificate of Origin

By making a contribution to this project, I certify that:

(a) The contribution was created in whole or in part by me and I have the right to submit it under the open source license indicated in the file; or

(b) The contribution is based upon previous work that, to the best of my knowledge, is covered under an appropriate open source license and I have the right under that license to submit that work with modifications, whether created in whole or in part by me, under the same open source license (unless I am permitted to submit under a different license), as indicated in the file; or

(c) The contribution was provided directly to me by some other person who certified (a), (b) or (c) and I have not modified it.

(d) I understand and agree that this project and the contribution are public and that a record of the contribution (including all personal information I submit with it, including my sign-off) is maintained indefinitely and may be redistributed consistent with this project or the open source license(s) involved.

## Best Practices

### Development Best Practices

- **Implement comprehensive logging and monitoring**: Use structured logging with appropriate log levels
- **Follow secure development practices**: Regular security reviews, vulnerability scanning, dependency updates
- **Use industry-standard containerization technologies**: Docker, Kubernetes, Helm
- **Implement proper error handling and recovery**: Graceful degradation and retry mechanisms
- **Design for offline and intermittent connectivity scenarios**: Local caching, queue-based processing

### Deployment Best Practices

**GitOps Implementation:**
- Use dedicated Git repositories for device configurations
- Implement proper access controls and audit logging
- Use signed commits for configuration changes
- Implement configuration validation before deployment

**Security Best Practices:**
- Rotate certificates and credentials regularly
- Implement network segmentation and firewall rules
- Use least-privilege access principles
- Monitor and audit all system activities

**Monitoring and Observability:**
- Implement comprehensive metrics collection
- Set up alerting for critical system events
- Use distributed tracing for complex workflows
- Regular health checks and system diagnostics

### Example Implementation Patterns

**Device Registration Flow:**

```python
# Example device registration implementation
class DeviceRegistration:
    def __init__(self, fleet_manager_url, device_cert):
        self.fleet_manager_url = fleet_manager_url
        self.device_cert = device_cert
        self.client = None
        
    async def register(self):
        # Download root CA certificate
        root_ca = await self.download_root_ca()
        
        # Create secure client
        self.client = SecureClient(
            base_url=self.fleet_manager_url,
            client_cert=self.device_cert,
            ca_cert=root_ca
        )
        
        # Prepare device capabilities
        capabilities = self.gather_device_capabilities()
        
        # Send onboarding request
        response = await self.client.post('/onboarding/device', {
            'apiVersion': 'onboarding.margo/v1',
            'kind': 'DeviceOnboarding',
            'deviceId': self.device_id,
            'certificate': base64.b64encode(self.device_cert).decode(),
            'capabilities': capabilities
        })
        
        # Store credentials for future use
        self.store_credentials(response)
        
        return response
```

**Application Deployment Monitoring:**

```python
# Example deployment monitoring implementation
class DeploymentMonitor:
    def __init__(self, device_client):
        self.device_client = device_client
        self.deployments = {}
        
    async def monitor_deployment(self, deployment_id):
        """Monitor deployment status and report to fleet manager"""
        deployment = self.deployments.get(deployment_id)
        if not deployment:
            return
            
        try:
            # Check deployment status
            status = await self.check_deployment_status(deployment)
            
            # Report status to fleet manager
            await self.device_client.post(
                f'/device/{self.device_id}/deployment/{deployment_id}/status',
                {
                    'apiVersion': 'deployment.margo/v1',
                    'kind': 'DeploymentStatus',
                    'deploymentId': deployment_id,
                    'status': status,
                    'components': deployment.components
                }
            )
            
        except Exception as e:
            # Report failure
            await self.report_deployment_failure(deployment_id, str(e))
```

## Documentation Generation Process

Some of the resources being used in the Margo APIs are being manually specified using MarkDown files. In this case mkdocs is used to generate the documentation in HTML format.

For others, the modeling language LinkML is being used to generate the documentation. In this case linkml is used to generate the MarkDown documents that are integrated with the above mentioned documents before mkdocs can generate the HTML documents.

### Documentation Structure

```
docs/
├── api/
│   ├── device-api.md
│   ├── onboarding-api.md
│   └── application-api.md
├── examples/
│   ├── device-registration.md
│   ├── application-deployment.md
│   └── observability-setup.md
├── security/
│   ├── authentication.md
│   ├── certificates.md
│   └── best-practices.md
└── compliance/
    ├── testing.md
    ├── certification.md
    └── checklist.md
```

### Building Documentation

```bash
# Install dependencies
pip install mkdocs mkdocs-material linkml

# Generate API documentation from LinkML schemas
linkml-docs generate --output docs/api/ schemas/

# Build HTML documentation
mkdocs build

# Serve documentation locally
mkdocs serve
```

---

## Copyright and License

Copyright © 2024 Margo

This specification is licensed under [appropriate open source license].

---

**Disclaimer:** This is a work-in-progress draft specification. Do not attempt to implement this version or reference it as authoritative. The specification is subject to change without notice.
