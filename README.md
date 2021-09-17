# Open Liberty on Azure Kubernetes Service Samples

## Overview

[Azure Kubernetes Service (AKS)](https://azure.microsoft.com/services/kubernetes-service/) deploys and manages containerized applications more easily with a fully managed Kubernetes service. Azure Kubernetes Service (AKS) offers serverless Kubernetes, an integrated continuous integration and continuous delivery (CI/CD) experience, and enterprise-grade security and governance. Unite your development and operations teams on a single platform to rapidly build, deliver, and scale applications with confidence.

[Open Liberty](https://openliberty.io) is an IBM Open Source project that implements the Eclipse MicroProfile specifications and is also Java/Jakarta EE compatible. Open Liberty is fast to start up with a low memory footprint and supports live reloading for quick iterative development. It is simple to add and remove features from the latest versions of MicroProfile and Java/Jakarta EE. Zero migration lets you focus on what's important, not the APIs changing under you.

This repository contains samples projects for deploying Java applications with Open Liberty on an Azure Kubernetes Service cluster.
These sample projects show how to use various features in Open Liberty and how to integrate with different Azure services.
Below table shows the list of samples available in this repository.

| Sample                           | Description                                | Guide                            |
|----------------------------------|--------------------------------------------|----------------------------------|
| [`open-liberty-operator-0.7.1`](open-liberty-operator-0.7.1) | Install Open Liberty Operator on an AKS cluster | [README.md](open-liberty-operator-0.7.1/README.md) |
| [`2-simple`](2-simple) | Deploy a simple Java EE application running on Open Liberty server to an AKS cluster. | [README.md](2-simple/README.md) |
| [`3-integration/aad-oidc`](3-integration/aad-oidc) | Extend [`2-simple`](2-simple) sample by integrating with Azure Active Directory OpenID Connect for security. | [README.md](3-integration/aad-oidc/README.md) |
| [`3-integration/connect-db`](3-integration/connect-db) | Extend [`2-simple`](2-simple) sample by integrating with Azure managed databases for data persistence. | TODO |
| [`3-integration/elk-logging`](3-integration/elk-logging) | Extend [`2-simple`](2-simple) sample by integrating with Elasticsearch stack for distributed logging. | TODO |
| [`4-finish`](4-finish) | A complete sample with all services integration including security, data persistence & distributed logging. | TODO |
