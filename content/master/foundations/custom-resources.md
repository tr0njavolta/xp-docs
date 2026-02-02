---
title: "Custom Resources & CRDs"
weight: 30
description: "Understanding Custom Resource Definitions and how to extend Kubernetes"
---

## Why Extend Kubernetes?

Kubernetes provides built-in resources like Pods, Services, and Deployments. But what if you need new resource types?

**Examples of needs:**
- Deploy applications with custom configuration specific to your organization
- Integrate with external systems (clouds, databases, monitoring)
- Implement custom business logic
- Create platform abstractions for your users

Without custom resources, you'd need to:
1. Write custom software outside Kubernetes
2. Manually manage resources
3. Handle monitoring and error recovery yourself

With custom resources and controllers, Kubernetes handles all of this.

## Custom Resource Definition (CRD)

A **Custom Resource Definition** defines the schema for a custom resource type.

### Basic CRD Example

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

### CRD Components

- **group**: Namespace for API versions (e.g., `example.com`, `aws.upbound.io`)
- **kind**: Singular name of the resource (e.g., `Database`)
- **plural**: Plural name (e.g., `databases`)
- **scope**: `Namespaced` (per-namespace) or `Cluster` (cluster-wide)
- **versions**: Supported API versions of this CRD
- **schema**: Validation rules for the resource

## Custom Resources

Once a CRD exists, you can create instances (custom resources):

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

Use standard kubectl commands:

```bash
# List all databases
kubectl get databases

# Get details
kubectl describe database production-db

# Watch for changes
kubectl watch database production-db

# Delete
kubectl delete database production-db
```

## Understanding .spec and .status

Every custom resource has:

### .spec - Desired State

What you want to exist:

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

### .status - Actual State

The controller updates this to show what's really happening:

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

## CRD Scope: Namespaced vs Cluster

### Namespaced (Default)

Resources exist in a specific namespace:

```yaml
scope: Namespaced
```

```yaml
apiVersion: example.com/v1
kind: Database
metadata:
  name: my-db
  namespace: team-a  # In team-a namespace
---
apiVersion: example.com/v1
kind: Database
metadata:
  name: my-db
  namespace: team-b  # Same name but different namespace
```

### Cluster Scope

Resources are global to the cluster:

```yaml
scope: Cluster
```

```yaml
apiVersion: example.com/v1
kind: Database
metadata:
  name: shared-db  # Must be unique across entire cluster
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

## Key Takeaways

- **CRDs extend Kubernetes** with custom resource types
- **Custom resources** are instances of a CRD
- **Controllers** implement the reconciliation logic
- **.spec** declares desired state
- **.status** shows actual state
- **Validation** ensures resources follow the schema
- **Scope** determines if resources are namespaced or cluster-wide

Understanding CRDs is essential for building and using Crossplane effectively.
