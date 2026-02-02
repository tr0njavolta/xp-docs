---
title: "Control Planes"
weight: 10
description: "Understanding what control planes are and how they work"
---

## What is a Control Plane?

A **control plane** is software that controls other software. It's a core pattern in cloud native systems and is used by all major cloud providers.

### How Control Planes Work

Control planes work by following a simple pattern:

1. **Declare Your Desired State**: You tell the control plane what you want through an API
2. **Continuous Monitoring**: The control plane continuously monitors the actual state
3. **Automatic Correction**: If the actual state drifts from desired state, the control plane automatically corrects it

This pattern is called **declarative** configuration or **infrastructure as code**.

### Real-World Example

Consider deploying an application:

**Without a Control Plane** (Imperative):
```bash
# You manually run these commands
docker run -d -p 8080:8080 myapp:v1
# If it crashes, you manually restart it
docker run -d -p 8080:8080 myapp:v1
# If you need to update, you manually kill and restart
docker kill <container-id>
docker run -d -p 8080:8080 myapp:v2
```

**With a Control Plane** (Declarative):
```yaml
# You declare what you want
apiVersion: v1
kind: Pod
metadata:
  name: myapp
spec:
  containers:
  - name: myapp
    image: myapp:v1
    ports:
    - containerPort: 8080
```

The control plane handles:
- Checking if the pod is running
- Automatically restarting it if it crashes
- Rolling out new versions when you change the image

### Key Characteristics

**Declarative**: You specify the desired end state, not the steps to achieve it.

**Automated**: The control plane automatically ensures the actual state matches your desired state.

**Self-Healing**: If something fails, the control plane automatically fixes it.

**Observable**: You can query the control plane to see the current state.

## Control Planes Manage Anything

Control planes aren't limited to containers. They can manage:
- Infrastructure (VMs, networks, databases)
- Applications (deployments, configuration)
- Cloud resources (S3 buckets, RDS instances, load balancers)
- Hybrid combinations of the above

## Kubernetes is a Control Plane

Kubernetes itself is a control plane. When you write a Kubernetes Deployment manifest, you're declaring desired state. Kubernetes ensures pods are running, restarts them if they fail, and rolls out updates.

Crossplane extends Kubernetes to be a control plane for infrastructure and other external systems.
