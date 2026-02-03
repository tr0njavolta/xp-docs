---
title: "Glossary"
weight: 60
description: "Understanding Custom Resource Definitions and how to extend Kubernetes"
---


{{<table>}}
| Term | Abbreviation | Definition |
| --- | --- | --- | --- |
| Composition | | Template for creating multiple Kubernetes resources as a single composite resource |
|Composite Resource | XR |  Custom API representing a set of resources as a single object |
|Composite Resource Definition | XRD |  Defines the schema for a custom API |
|Claim | XRC | Namespace-scoped version of Composite Resources (deprecated in v2.0+) |
|Managed Resource | MR | Kubernetes custom resource representing an external service |
|Provider | | Software enabling Crossplane to provision infrastructure on external services |
|Package | | OCI container image containing Providers, Functions, or Configurations |
|Function | | Extensions that template Crossplane resources in compositions |
|Configuration | | Portable package containing Compositions, XRDs, and dependencies |
|External Resource | | Actual resource created in external provider (AWS, Azure, GCP) |
|Control Plane | | Software that controls other software via declarative configuration |
|Provider Config | | Settings for Provider authentication and communication |
|Connection Details | | Resource-specific details like passwords, endpoints, connection strings |
|Environment Config | | Cluster-scoped ConfigMap-like resource for Compositions|
|Composition Revision | | Immutable snapshot of a Composition at a point in time|
|Composed Resource | | Resource created by a composite resource |
|Observed State | | Actual current state that Crossplane observes |
|Desired State | | Changes the function pipeline wants to make |
|Management Policies | | Settings determining which actions Crossplane can take|
| ConfigMap | | |

{{</table>}}
