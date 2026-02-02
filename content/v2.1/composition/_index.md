---
title: Composition
weight: 51
description: Build custom APIs by composing Kubernetes resources
---

## What is Composition?

**Composition** is the practice of combining multiple resources into a new, higher-level resource. Instead of exposing the complexity of many resources, you create a simpler abstraction that hides the details.

### Real-World Analogy

Think of a restaurant:
- **Low-level resources**: Chef, ingredients, equipment, procedures
- **Composed resource**: A dish (e.g., "Pasta Carbonara") that hides all the complexity

When you order pasta carbonara, you don't specify:
- Which chef will cook it
- How long each ingredient should cook
- What temperature the pan should be

You just order "Pasta Carbonara" and the restaurant handles the details.

## Composition in Crossplane

In Crossplane, composition means:

1. **Define a custom resource** that represents your abstraction
2. **Define a composition** that specifies what resources get created
3. **Users create instances** of your custom resource
4. **Resources are automatically created** according to the composition

## Example: App Composition

### Step 1: Define Your Custom Resource (XRD)

```yaml
apiVersion: apiextensions.crossplane.io/v2
kind: CompositeResourceDefinition
metadata:
  name: apps.example.com
spec:
  group: example.com
  names:
    kind: App
    plural: apps
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
              image:
                type: string
                description: "Container image for the app"
              replicas:
                type: integer
                description: "Number of replicas"
```

### Step 2: Define the Composition

```yaml
apiVersion: apiextensions.crossplane.io/v1
kind: Composition
metadata:
  name: app-with-db
spec:
  compositeTypeRef:
    apiVersion: example.com/v1
    kind: App
  resources:
  - name: deployment
    base:
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: app
      spec:
        replicas: 3
        template:
          spec:
            containers:
            - name: app
              image: nginx:1.14.2
  - name: database
    base:
      apiVersion: s3.aws.upbound.io/v1beta1
      kind: Bucket
      metadata:
        name: app-bucket
      spec:
        forProvider:
          region: us-east-1
```

### Step 3: User Creates an App

```yaml
apiVersion: example.com/v1
kind: App
metadata:
  name: my-app
spec:
  image: my-app:1.0.0
  replicas: 3
```

### Step 4: Resources Are Created Automatically

Crossplane automatically creates:
- A Deployment with 3 replicas running your image
- An S3 bucket for storage
- Any other resources specified in the composition

User sees a simple `App` resource. Behind the scenes, multiple resources are working together.

## Composition Benefits

### Abstraction

Hide complexity:
```
User perspective:
  Create 1 App resource

Behind the scenes:
  - Deployment (running your code)
  - Service (networking)
  - ConfigMap (configuration)
  - Secret (credentials)
  - Storage resources
  - Database
  - Monitoring
  - etc.
```

### Consistency

Everyone creates apps the same way:
```yaml
apiVersion: example.com/v1
kind: App
metadata:
  name: app-1
spec:
  image: app-1:1.0.0
---
apiVersion: example.com/v1
kind: App
metadata:
  name: app-2
spec:
  image: app-2:1.0.0
```

Both apps get the same infrastructure, avoiding snowflake configurations.

### Simplification

Complex infrastructure becomes simple:
```yaml
# Your abstraction (simple)
apiVersion: example.com/v1
kind: App
metadata:
  name: my-app
spec:
  image: my-app:1.0.0

# Replaces hundreds of lines of Kubernetes YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app-deployment
spec:
  replicas: 3
  template:
    spec:
      containers:
      - name: my-app
        image: my-app:1.0.0
      - name: sidecar
        image: monitoring-sidecar:1.0.0
---
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  # ... service config ...
---
# ... many more resources ...
```

## Composition Patterns

### 1. Compose Kubernetes Resources

Create a custom resource that composes Kubernetes resources:

```yaml
apiVersion: example.com/v1
kind: App
metadata:
  name: my-app
spec:
  image: my-app:1.0.0

# Composition creates:
# - Deployment
# - Service
# - ConfigMap
# - PersistentVolume
```

### 2. Compose Managed Resources

Create a custom resource that composes cloud infrastructure:

```yaml
apiVersion: example.com/v1
kind: Database
metadata:
  name: prod-db
spec:
  engine: postgres
  storage: 100Gi

# Composition creates:
# - RDS Instance
# - Security Group
# - Subnet Group
# - Parameter Group
# - Backup configuration
```

### 3. Compose Mixed Resources

Combine Kubernetes and cloud resources:

```yaml
apiVersion: example.com/v1
kind: WebApp
metadata:
  name: my-webapp
spec:
  image: webapp:1.0.0
  dbEngine: postgres

# Composition creates:
# - Kubernetes Deployment
# - Kubernetes Service
# - AWS RDS Database
# - AWS S3 Bucket
# - ConfigMap with connection details
```

## Composition with Functions

Modern Crossplane compositions use **functions** to add logic:

```yaml
apiVersion: apiextensions.crossplane.io/v1
kind: Composition
metadata:
  name: app-with-logic
spec:
  compositeTypeRef:
    apiVersion: example.com/v1
    kind: App
  mode: Pipeline
  pipeline:
  - step: process-user-input
    functionRef:
      name: function-go-templating
    input:
      apiVersion: gotemplating.fn.crossplane.io/v1beta1
      kind: GoTemplate
      spec:
        source: Inline
        inline:
          template: |
            - name: deployment
              base:
                apiVersion: apps/v1
                kind: Deployment
              # Template can access .observed.composite.resource.spec fields
  - step: validate-and-patch
    functionRef:
      name: function-patch-and-transform
```

Functions allow you to:
- Use template logic
- Validate inputs
- Transform values
- Implement custom business logic
- Support multiple languages (YAML, KCL, Python, Go)

## When to Use Composition

**Use composition when you want to:**
- Hide infrastructure complexity from users
- Ensure consistent deployments
- Enforce organizational standards
- Provide self-service infrastructure
- Manage infrastructure as code

**Don't over-compose if:**
- Users need direct control over individual resources
- Each deployment is unique
- The abstraction adds no value

{{< auto-index >}}
