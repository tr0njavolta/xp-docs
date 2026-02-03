---
title: "Custom Resources & CRDs"
weight: 30
description: "Understanding Custom Resource Definitions and how to extend Kubernetes"
---

**Previous:** [Kubernetes Basics]({{<ref "kubernetes-basics">}}) - Essential
concepts including reconciliation loops and controllers

---

Kubernetes **resources** are endpoints in the Kubernetes API and contains the
collections for specific resource `kinds`.

Resources like Pods, Services, and Deployments are built-in to the Kubernetes
API. **Custom resources** extend the Kubernetes API beyond these built in
objects. **Custom resource definitions** are Kubernetes API resources that allow
you to define custom resources in your Kubernetes clusters.

For example, if you manage a Kubernetes cluster and need to integrate it with an
external system, you can create a CRD to define the schema of that system and
allow Kubernetes to manage it for you.


## Basic CRD Example

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: databases.example.com
spec:
  group: example.com
  names:
    kind: Database
    plural: databases
  scope: Namespaced
  versions:
  - name: v1
    served: true
    storage: true
    schema:
      openAPIV3Schema:
        type: object
        properties:
          spec:
            type: object
            properties:
              engine:
                type: string
                description: "Database engine (postgres, mysql, etc)"
              version:
                type: string
                description: "Database version"
              size:
                type: string
                description: "Database size (small, medium, large)"
```

### CRD components

- **group**: API Group for organizing related APIs (e.g., `example.com`, `aws.upbound.io`)
- **kind**: Singular name of the resource (e.g., `Database`)
- **plural**: Plural name (e.g., `databases`)
- **scope**: Namespaced (per-namespace) or `Cluster` (cluster-wide)
- **versions**: Supported API versions of this CRD
- **schema**: Validation rules for the resource

## Custom Resources

Once a CRD exists, you can create custom resources that reference the CRD you
created:

```yaml
apiVersion: example.com/v1
kind: Database
metadata:
  name: production-db
  namespace: my-app
spec:
  engine: postgres
  version: "14"
  size: large
```


### Understanding .spec and .status

Every custom resource has:

### .spec

The **spec** field contains the desired state - what you want to exist:

```yaml
apiVersion: example.com/v1
kind: Database
metadata:
  name: production-db
spec:
  engine: postgres      # What you want
  version: "14"         # What you want
  size: large           # What you want
```

### .status

The **status** field shows the observed state - the controller updates this to
show what's really happening:

```yaml
apiVersion: example.com/v1
kind: Database
metadata:
  name: production-db
spec:
  engine: postgres
  version: "14"
  size: large
status:                 # Updated by controller
  phase: Creating       # Current state
  conditions:
  - type: Ready
    status: "False"
    reason: "Provisioning"
    message: "Database is being created"
  connectionString: ""  # Not ready yet
  endpoint: ""          # Not ready yet
```

As the controller works:

```yaml
status:                 # Updated by controller
  phase: Ready          # Now running
  conditions:
  - type: Ready
    status: "True"
    reason: "Available"
    message: "Database is ready"
  connectionString: "postgres://user:pass@prod-db.example.com:5432/db"
  endpoint: "prod-db.example.com"
```

## Controllers Make It Work

A **controller** is software that:
1. Watches custom resources
2. Takes action when they're created/updated
3. Updates the `.status` field to report progress

Example Database Controller behavior:

```
USER CREATES: Database resource
        ↓
CONTROLLER WATCHES: "New database resource!"
        ↓
CONTROLLER ACTS: Provisions database in cloud
        ↓
CONTROLLER UPDATES: .status.phase = "Provisioning"
        ↓
CONTROLLER POLLS: Is database ready yet?
        ↓
DATABASE READY: Controller detects it's ready
        ↓
CONTROLLER UPDATES: .status.phase = "Ready"
        ↓
CONTROLLER UPDATES: Sets .status.endpoint, .status.connectionString
        ↓
USER CHECKS: kubectl describe database production-db
USER SEES: Status shows "Ready" and connection details
```

## Validating Schemas

CRDs can validate resource definitions using JSON Schema:

```yaml
schema:
  openAPIV3Schema:
    type: object
    properties:
      spec:
        type: object
        required:        # These fields are required
        - engine
        - size
        properties:
          engine:
            type: string
            enum:        # Only allow these values
            - postgres
            - mysql
            - mariadb
          size:
            type: string
            enum:
            - small
            - medium
            - large
          replicas:
            type: integer
            minimum: 1   # Must be at least 1
            maximum: 5   # Can't be more than 5
```

Invalid resources are rejected:

```bash
$ kubectl apply -f invalid-database.yaml
error: resource "databases.example.com/production-db" is invalid: ...
```

## How Crossplane Uses CRDs

Crossplane uses custom resources to:
- Define **composite resources** (your custom APIs)
- Represent **managed resources** (cloud infrastructure)
- Manage **compositions** (how resources are created)
- Implement **operations** (operational workflows)

The fundamental pattern is the same:
1. Define a CRD describing your resource
2. Create instances of that resource
3. A controller watches and makes it happen
4. Status shows what's actually happening

Understanding CRDs is essential for building and using Crossplane effectively.

---

## Next steps

Congratulations! You've completed the Foundations section and now understand the core concepts behind Crossplane.

**Ready to build with Crossplane?** Check out the [Get Started]({{<ref "../get-started">}}) guides to:

-   Build your first control plane with [Composition]({{<ref "../get-started/get-started-with-composition">}})
-   Manage cloud infrastructure with [Managed Resources]({{<ref "../get-started/get-started-with-managed-resources">}})
-   Create operational workflows with [Operations]({{<ref "../get-started/get-started-with-operations">}})
