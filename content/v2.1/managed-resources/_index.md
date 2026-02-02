---
title: Managed Resources
weight: 52
description: Manage cloud resources in Kubernetes
---

## What are Managed Resources?

**Managed resources** are Kubernetes custom resources that represent cloud infrastructure and external systems. They allow you to manage anything outside your Kubernetes cluster using standard Kubernetes tools.

Instead of logging into AWS, Azure, GCP, or other cloud providers, you manage infrastructure with `kubectl`.

## The Problem Managed Resources Solve

### Before Managed Resources

To create infrastructure, you'd:
```
User → kubectl
    ↓
Kubernetes cluster
    (can only manage Kubernetes resources)
    ↓
User must manually log into AWS/Azure/GCP
    ↓
Create resources manually using web UI or CLI
    ↓
No coordination between Kubernetes and infrastructure
    ↓
No GitOps (can't version control infrastructure as code)
    ↓
Hard to manage lifecycle (updates, deletion, etc.)
```

### With Managed Resources

```
User → kubectl
    ↓
Kubernetes cluster
    ↓
Managed Resources represent infrastructure
    ↓
Crossplane provider automatically creates/updates infrastructure
    ↓
Everything is code, version controlled, and automated
```

## Managed Resource Example

### Creating an AWS S3 Bucket

Without Managed Resources (manual):
```bash
# Log into AWS console or AWS CLI
aws s3 mb s3://my-bucket --region us-east-1
# Then somehow tell your Kubernetes cluster about it
```

With Managed Resources (declarative):
```yaml
apiVersion: s3.aws.upbound.io/v1beta1
kind: Bucket
metadata:
  name: my-bucket
spec:
  forProvider:
    region: us-east-1
```

Apply it:
```bash
kubectl apply -f bucket.yaml
```

Crossplane automatically:
- Creates the S3 bucket in AWS
- Updates the resource's status as it progresses
- Reports any errors
- Watches for changes and corrects drift

## Managed Resources Structure

### Spec (Desired State)

```yaml
apiVersion: s3.aws.upbound.io/v1beta1
kind: Bucket
metadata:
  name: my-bucket
spec:
  forProvider:
    # Cloud-specific configuration
    region: us-east-1
    acl: "private"
    serverSideEncryptionConfiguration:
    - rule:
      - applyServerSideEncryptionByDefault:
          sseAlgorithm: AES256
  deletionPolicy: Delete  # What to do when resource is deleted
  managementPolicy: FullControl  # How much to manage
```

### Status (Actual State)

```yaml
status:
  conditions:
  - type: Ready
    status: "True"
    reason: "Success"
  - type: Synced
    status: "True"
  atProvider:
    # Cloud-reported state
    arn: "arn:aws:s3:::my-bucket"
    domainName: "my-bucket.s3.amazonaws.com"
    region: "us-east-1"
```

## Providers

A **provider** is a Crossplane extension that knows how to manage resources for a specific system.

### Popular Providers

- **AWS Provider**: EC2, RDS, S3, DynamoDB, etc.
- **Azure Provider**: VMs, Databases, Storage, etc.
- **GCP Provider**: Compute, Cloud SQL, Storage, etc.
- **Terraform Provider**: Any infrastructure Terraform supports
- **Kubernetes Provider**: Create Kubernetes resources in other clusters
- **GitHub Provider**: Repositories, teams, actions, etc.

### Installing a Provider

```yaml
apiVersion: pkg.crossplane.io/v1
kind: Provider
metadata:
  name: provider-aws-s3
spec:
  package: xpkg.upbound.io/upbound/provider-aws-s3:v1.2.0
```

Once installed, you can use all the managed resources that provider supports.

## Managed Resource Lifecycle

### Creation

```yaml
apiVersion: s3.aws.upbound.io/v1beta1
kind: Bucket
metadata:
  name: my-bucket
```

When you apply this, the provider's controller:
1. Detects the new resource
2. Calls the AWS API to create the bucket
3. Updates `.status.conditions[Ready]` as it works
4. Reports any errors

### Updates

Change the spec:
```yaml
spec:
  forProvider:
    region: us-east-1
    acl: "public-read"  # Changed from "private"
```

The provider:
1. Compares desired vs actual state
2. Calls AWS API to update the bucket
3. Updates status

### Drift Detection

If someone manually changes the bucket in AWS:

```
AWS bucket: acl = "public-read"
Kubernetes desired: acl = "private"
        ↓
Provider detects drift
        ↓
Provider fixes it (changes back to "private")
        ↓
Automatic self-healing
```

### Deletion

```bash
kubectl delete bucket my-bucket
```

Based on the `deletionPolicy`:
- **Delete**: Remove from cloud provider
- **Orphan**: Leave in cloud provider, only remove Kubernetes resource

## Connecting to Cloud Providers

Managed resources need credentials to access cloud providers.

### AWS Example

Create a secret with credentials:
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: aws-credentials
  namespace: crossplane-system
type: Opaque
stringData:
  credentials: |
    [default]
    aws_access_key_id = YOUR_KEY
    aws_secret_access_key = YOUR_SECRET
```

Configure the provider to use it:
```yaml
apiVersion: aws.upbound.io/v1beta1
kind: ProviderConfig
metadata:
  name: default
spec:
  credentials:
    source: Secret
    secretRef:
      namespace: crossplane-system
      name: aws-credentials
      key: credentials
```

## Managed Resources in Compositions

Managed resources are most powerful when used in compositions:

```yaml
apiVersion: apiextensions.crossplane.io/v1
kind: Composition
metadata:
  name: wordpress
spec:
  compositeTypeRef:
    apiVersion: example.com/v1
    kind: WordPress
  resources:
  - name: database
    base:
      apiVersion: rds.aws.upbound.io/v1beta1
      kind: Instance
      spec:
        forProvider:
          engine: mysql
          allocatedStorage: 20
  - name: storage
    base:
      apiVersion: s3.aws.upbound.io/v1beta1
      kind: Bucket
      spec:
        forProvider:
          region: us-east-1
  - name: app
    base:
      apiVersion: apps/v1
      kind: Deployment
      spec:
        # Deployment config
```

User creates:
```yaml
apiVersion: example.com/v1
kind: WordPress
metadata:
  name: my-blog
```

Crossplane automatically creates:
- RDS database
- S3 bucket
- Kubernetes deployment
- Networking, security groups, etc.

## Managed Resources vs Manual Management

| Aspect | Manual | Managed Resources |
|--------|--------|------------------|
| **Version Control** | Difficult | Yes, everything is YAML |
| **Automation** | Manual steps | Fully automated |
| **Disaster Recovery** | Manual restore | Recreate with kubectl |
| **Audit Trail** | Limited | Complete (git history) |
| **Multi-Cloud** | Different tools per cloud | Same kubectl commands |
| **Team Collaboration** | Scattered knowledge | Documented in code |
| **Infrastructure as Code** | Partial | Complete |

{{< auto-index >}}
