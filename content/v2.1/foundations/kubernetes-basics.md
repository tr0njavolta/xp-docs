---
title: "Kubernetes Basics"
weight: 20
description: "Essential Kubernetes concepts: YAML, custom resources, and reconciliation loops"
---

**Previous:** [Control Planes]({{<ref "control-planes">}}) - Understanding declarative configuration and control plane patterns

---

Understanding how to interact with Kubernetes is essential to understanding
Crossplane.  

Kubernetes uses YAML manifests to declare desired state. Parsing and writing
YAML manifests will help you in your Crossplane journey.

## Basic YAML Structure

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

## Namespaces

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

## Reconciliation loops

Kubernetes uses **reconciliation loops** to ensure the actual state matches the
desired state. This continuous state observation is a key benefit of [control
planes]({{<ref "control-planes">}}).

For example, to deploy an `nginx` web app, you create a YAML manifest to declare
all your desired resources:

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

The Kubernetes controller performs continuous observation on the external
resource, compares the actual state to the desired state, adjusts the resources
based on what it observes.

In this example, if you declare you need 3 replicas and Kubernetes only observes
2 after a pod crash, the Controller creates a new pod and repeats the
observe/compare/correct cycle.

## Controllers

A **controller** is the software that implements reconciliation for a resource
type. Controllers watch resources and take action when changes occur.

Controllers implement the reconciliation for a resource type. Controllers observe
the resources and updates them when a change occurs.

Kubernetes built-in controllers include Deployment Controllers, Node
Controllers, and ReplicaSet controllers to observe and manage those resources.

## Resource Status

Resources have two important sections:

- **spec**: The desired state - what you declare in your Custom Resource
- **status**: The observed state - real resources as observed

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

---

## Next steps

Now that you understand Kubernetes basics, learn how to extend Kubernetes with custom resources:

**Next:** [Custom Resources & CRDs]({{<ref "custom-resources">}}) - Learn how to extend Kubernetes with Custom Resource Definitions and create your own resource types
