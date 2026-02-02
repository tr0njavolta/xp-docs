---
title: "Kubernetes Basics"
weight: 20
description: "Essential Kubernetes concepts: YAML, custom resources, and reconciliation loops"
---

## Kubernetes YAML Syntax

Kubernetes uses YAML manifests to declare desired state. Understanding YAML is essential for working with Crossplane.

### Basic YAML Structure

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
```

Key YAML components:

- **apiVersion**: API version of the resource (e.g., `v1`, `apps/v1`)
- **kind**: Type of resource (e.g., `Pod`, `Deployment`, `Service`)
- **metadata**: Information about the resource (name, namespace, labels, etc.)
- **spec**: Desired specification for the resource

### Namespaces

Kubernetes uses **namespaces** to partition resources within a cluster.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  namespace: my-app  # This pod lives in the "my-app" namespace
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
```

Default namespace is `default`. To see resources in a namespace:

```bash
kubectl get pods -n my-app
```

## Kubernetes Custom Resources

Kubernetes allows you to extend it with **custom resources** - custom resource definitions (CRDs) that define new types beyond the built-in Pod, Deployment, Service, etc.

### Custom Resource Definition (CRD)

A CRD is a template that defines the structure of a custom resource:

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: myapps.example.com
spec:
  names:
    kind: MyApp
    plural: myapps
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
              replicas:
                type: integer
              image:
                type: string
```

### Using Custom Resources

Once a CRD is defined, you can create instances of it:

```yaml
apiVersion: example.com/v1
kind: MyApp
metadata:
  name: my-app
spec:
  replicas: 3
  image: myapp:1.0.0
```

Get custom resources:

```bash
kubectl get myapps
kubectl describe myapp my-app
```

## Reconciliation Loops

Kubernetes uses **reconciliation loops** to ensure the actual state matches the desired state. This is the heart of control planes.

### How Reconciliation Works

```
┌─────────────────────────────────────┐
│ Reconciliation Loop                 │
└─────────────────────────────────────┘
        ↓
  1. OBSERVE: Read current state
        ↓
  2. COMPARE: Desired vs Actual
        ↓
  3. DECIDE: What needs to change
        ↓
  4. ACT: Make changes to reach desired state
        ↓
  5. REPEAT: Continuously watch for drift
```

### Example: Deployment Reconciliation

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 3  # Desired state: 3 replicas
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: web
        image: nginx:1.14.2
```

The Kubernetes controller continuously:
1. **Observes**: Checks how many web-app pods are currently running
2. **Compares**: Sees that 2 are running, but 3 are desired
3. **Decides**: Need to start 1 more pod
4. **Acts**: Creates a new pod
5. **Repeats**: Continuously checks if the state matches

If a pod crashes, the reconciliation loop will automatically restart it.

## Controllers

A **controller** is the software that implements reconciliation for a resource type. Controllers watch resources and take action when changes occur.

For example:
- **Deployment Controller**: Watches Deployments and ensures the right number of pods are running
- **StatefulSet Controller**: Manages stateful applications with persistent identity
- **Service Controller**: Manages how traffic is routed to pods

## Resource Status

Resources have two important sections:

- **spec**: The desired state (what you want)
- **status**: The actual state (what's currently happening)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
status:
  phase: Running
  containerStatuses:
  - name: nginx
    ready: true
    state:
      running:
        startedAt: "2024-02-01T10:30:00Z"
```

You can observe status:

```bash
kubectl get pod my-pod -o yaml | grep -A 10 status:
```

## Key Takeaways

- **Declarative**: You declare desired state in YAML, not steps
- **Namespaces**: Isolate resources within a cluster
- **Custom Resources**: Extend Kubernetes with your own resource types
- **Reconciliation**: Controllers continuously ensure actual state matches desired state
- **Self-Healing**: If something fails, the reconciliation loop fixes it automatically
- **Status**: Always shows the current state of resources

These concepts are fundamental to understanding how Crossplane works.
