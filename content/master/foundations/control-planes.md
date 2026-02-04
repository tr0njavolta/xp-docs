---
title: "Control Planes"
weight: 10
description: "Understanding what control planes are and how they work"
---

A **control plane** is software that controls other software. It's a core
pattern in cloud native systems and is used by all major cloud providers.

## How Control Planes Work

Control planes operate in a standardized workflow:

1. **Declare Your Desired State**: You tell the control plane what you want through an API
2. **Continuous Monitoring**: The control plane continuously monitors the actual state
3. **Automatic Correction**: If the actual state drifts from desired state, the control plane automatically corrects it

Control planes enable **declarative** configuration. You declare your desired
state for your resources and the control plane manages the actual state.

For example, you can manually run commands to deploy an application in Docker:
```bash
# You manually run these commands
docker run -d -p 8080:8080 myapp:v1
# If it crashes, you manually restart it
docker run -d -p 8080:8080 myapp:v1
# If you need to update, you manually kill and restart
docker kill <container-id>
docker run -d -p 8080:8080 myapp:v2
```

With a declarative control plane, you write the end state of your application:

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

## Next steps

Now that you understand control planes, learn about the Kubernetes concepts that
power Crossplane:

* [Kubernetes Basics]({{<ref "kubernetes-basics">}}) - Essential Kubernetes concepts including YAML, reconciliation loops, and controllers

