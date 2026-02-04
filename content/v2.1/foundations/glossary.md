---
title: "Glossary"
weight: 80
description: "Reference guide for Crossplane and Kubernetes terminology"
---

This glossary provides definitions for key terms used throughout the Crossplane
documentation.

{{< table "table table-striped" >}}
| Term | Abbreviation | Definition |
| --- | --- | --- | --- |
| API Group | | Namespace for organizing related Kubernetes APIs (e.g., apps/v1, example.com). See [Custom Resources & CRDs]({{<ref "custom-resources#crd-components">}}) |
| Claim | XRC | Namespace-scoped version of Composite Resources (deprecated in v2.0+) |
| Composed Resource | | Resource created by a composite resource |
| Composition | | Template for creating multiple Kubernetes resources as a single composite resource |
| Composition Revision | | Immutable snapshot of a Composition at a point in time|
| Composite Resource | XR | Custom API representing a set of resources as a single object |
| Composite Resource Definition | XRD | Defines the schema for a custom API |
| ConfigMap | | Kubernetes resource for storing configuration data as key-value pairs |
| Configuration | | Portable package containing Compositions, XRDs, and dependencies |
| Connection Details | | Resource-specific details like passwords, endpoints, connection strings |
| Control Plane | | Software that controls other software via declarative configuration. Learn more: [Control Planes]({{<ref "control-planes">}}) |
| Controller | | Software that watches Kubernetes resources and reconciles actual state with desired state. Learn more: [Kubernetes Basics]({{<ref "kubernetes-basics#controllers">}}) and [Custom Resources & CRDs]({{<ref "custom-resources#controllers-make-it-work">}}) |
| Custom Resource | CR | User-defined Kubernetes resource type created by a Custom Resource Definition. Learn more: [Custom Resources & CRDs]({{<ref "custom-resources">}}) |
| Custom Resource Definition | CRD | Kubernetes resource that defines a new custom resource type and its schema. Learn more: [Custom Resources & CRDs]({{<ref "custom-resources">}}) |
| Declarative Configuration | | Describing the desired end state rather than the steps to achieve it. Learn more: [Control Planes]({{<ref "control-planes#how-control-planes-work">}}) |
| Desired State | | The configuration you want; defined in the spec field of a resource. Learn more: [Kubernetes Basics]({{<ref "kubernetes-basics#resource-status">}}) and [Custom Resources & CRDs]({{<ref "custom-resources#spec---desired-state">}}) |
| Drift | | When actual state diverges from desired state. Learn more: [Control Planes]({{<ref "control-planes">}}) |
| Environment Config | | Cluster-scoped ConfigMap-like resource for Compositions|
| External Resource | | Actual resource created in external provider (AWS, Azure, GCP) |
| Function | | Extensions that template Crossplane resources in compositions |
| Managed Resource | MR | Kubernetes custom resource representing an external service |
| Management Policies | | Settings determining which actions Crossplane can take|
| Namespace | | Kubernetes mechanism for partitioning resources within a cluster. Learn more: [Kubernetes Basics]({{<ref "kubernetes-basics#namespaces">}}) |
| Observed State | | Actual current state; reported in the status field of a resource. Learn more: [Kubernetes Basics]({{<ref "kubernetes-basics#resource-status">}}) and [Custom Resources & CRDs]({{<ref "custom-resources#status---actual-state">}}) |
| Package | | OCI container image containing Providers, Functions, or Configurations |
| Provider | | Software enabling Crossplane to provision infrastructure on external services |
| Provider Config | | Settings for Provider authentication and communication |
| Reconciliation Loop | | Continuous process of observing actual state, comparing to desired state, and taking corrective action. Learn more: [Kubernetes Basics]({{<ref "kubernetes-basics#reconciliation-loops">}}) |
| Schema | | Structure and validation rules for a resource, defined using OpenAPI v3. Learn more: [Custom Resources & CRDs]({{<ref "custom-resources#validating-schemas">}}) |
| spec | | Field in a Kubernetes resource containing the desired state configuration. Learn more: [Kubernetes Basics]({{<ref "kubernetes-basics#resource-status">}}) and [Custom Resources & CRDs]({{<ref "custom-resources#spec---desired-state">}}) |
| status | | Field in a Kubernetes resource containing the actual state and condition information. Learn more: [Kubernetes Basics]({{<ref "kubernetes-basics#resource-status">}}) and [Custom Resources & CRDs]({{<ref "custom-resources#status---actual-state">}}) |
{{</table>}}

---


