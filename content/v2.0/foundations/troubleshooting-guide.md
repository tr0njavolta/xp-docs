---
title: "Troubleshooting Guide"
weight: 65
description: "Common issues and how to fix them when working with Crossplane"
---

## Troubleshooting Crossplane

This guide helps you diagnose and fix common Crossplane issues.

## Installation Issues

### Issue: Crossplane pods not starting

**Symptom:**
```
$ kubectl get pods -n crossplane-system
NAME                                       READY   STATUS             RESTARTS   AGE
crossplane-6d67f8cd9d-g2gjw                0/1     CrashLoopBackOff   5          5m
crossplane-rbac-manager-86d9b5cf9f-2vc4s   1/1     Running            0          5m
```

**How to diagnose:**
```bash
# Check the logs
kubectl logs -n crossplane-system deployment/crossplane

# Check events
kubectl describe pod -n crossplane-system <pod-name>

# Check resource requests vs available
kubectl top nodes
kubectl describe nodes
```

**Common causes:**
- Insufficient memory (need at least 2 GB cluster RAM)
- Incompatible Kubernetes version
- RBAC issues

**Fix:**
- Verify cluster has minimum resources: `kubectl top nodes`
- Check Kubernetes version: `kubectl version`
- For RBAC issues, reinstall with RBAC manager enabled

### Issue: Helm install fails

**Symptom:**
```
Error: INSTALLATION FAILED: ... chart requires kubeVersion: ...
```

**How to diagnose:**
```bash
kubectl version
helm version
```

**Common causes:**
- Kubernetes version too old or too new
- Helm version is outdated (need v3.2.0+)

**Fix:**
- Upgrade Kubernetes or Helm as needed
- Check [official documentation](https://kubernetes.io/docs/setup/)

---

## Composition Issues

### Issue: Composite Resource Definition (XRD) not established

**Symptom:**
```
$ kubectl get xrd apps.example.crossplane.io
NAME                         ESTABLISHED   OFFERED   AGE
apps.example.crossplane.io   False                   2m
```

**How to diagnose:**
```bash
# Check XRD status
kubectl describe xrd apps.example.crossplane.io

# Check Crossplane logs
kubectl logs -n crossplane-system deployment/crossplane | grep -i xrd
```

**Common causes:**
- XRD YAML has syntax errors
- Schema validation failed
- Crossplane can't parse the XRD

**Fix:**
- Validate YAML: `kubectl apply -f xrd.yaml --dry-run=client`
- Check schema in XRD for required fields
- Review Crossplane logs for specific error messages

### Issue: Composition not being used

**Symptom:**
```
$ kubectl create -f app.yaml
$ kubectl describe app my-app
Status: <empty>
```

The composite resource is created but nothing happens.

**How to diagnose:**
```bash
# Check if composition is installed
kubectl get composition

# Check composition status
kubectl describe composition app-yaml

# Check XRD references correct composition
kubectl get xrd apps.example.crossplane.io -o yaml
```

**Common causes:**
- Composition not installed
- Composition name doesn't match XRD reference
- Function not installed/healthy

**Fix:**
- Install composition: `kubectl apply -f composition.yaml`
- Verify composition name in XRD matches
- Check function status: `kubectl get functions`

### Issue: Composition function fails

**Symptom:**
```
$ kubectl describe app my-app
Status:
  Conditions:
  - Type: Ready
    Status: "False"
    Reason: "CompositionFunctionError"
    Message: "function returned error"
```

**How to diagnose:**
```bash
# Check function status
kubectl get functions
kubectl describe function <function-name>

# Check function logs
kubectl logs -n crossplane-system deployment/<function-name>

# Get more detailed error
kubectl describe composite app my-app
```

**Common causes:**
- Function pod crashed or isn't healthy
- Function has incorrect configuration
- Function input YAML is malformed
- Function requires specific version

**Fix:**
- Ensure function is installed and healthy: `kubectl get functions`
- Check function logs for error details
- Verify function input YAML syntax
- Check Crossplane documentation for function requirements

### Issue: Patches not applying correctly

**Symptom:**
```
Composition defines patches but composed resources don't have expected values
```

**How to diagnose:**
```bash
# Check composed resource
kubectl get deployments -A
kubectl describe deployment <composed-resource>

# Compare to composition
kubectl get composition <name> -o yaml
```

**Common causes:**
- Patch field path doesn't exist
- Patch transform has wrong syntax
- Order of patches matters

**Fix:**
- Verify field paths exist in target resource
- Check patch transform syntax in documentation
- Test patch with simple example first

---

## Managed Resources Issues

### Issue: Provider not installing

**Symptom:**
```
$ kubectl get providers
NAME                                     INSTALLED   HEALTHY
crossplane-contrib-provider-aws-s3       False       False
```

**How to diagnose:**
```bash
# Check provider status
kubectl describe provider <provider-name>

# Check provider logs
kubectl logs -n crossplane-system deployment/<provider-name>

# Check available disk space
kubectl get persistentvolumeclaims -A
df -h
```

**Common causes:**
- Provider image pull failed (network/credentials)
- Insufficient disk space
- Provider version incompatible with Crossplane
- RBAC permissions insufficient

**Fix:**
- Check network connectivity
- Verify Docker credentials for private registries
- Check disk space: `df -h`
- Verify provider version compatibility
- Check RBAC: `kubectl get clusterrolebinding | grep crossplane`

### Issue: Can't connect to cloud provider

**Symptom:**
```
$ kubectl describe bucket my-bucket
Status:
  Conditions:
  - Type: Synced
    Status: "False"
    Reason: "CannotConnectToProvider"
```

**How to diagnose:**
```bash
# Check provider config exists
kubectl get providerconfig

# Check credentials secret
kubectl get secret -n crossplane-system <provider-secret>

# Check provider config references correct secret
kubectl get providerconfig -o yaml
```

**Common causes:**
- Credentials secret not created
- ProviderConfig not configured
- Credentials are invalid or expired
- ProviderConfig doesn't reference correct secret

**Fix:**
- Create credentials secret: `kubectl create secret generic aws-creds --from-file=credentials=~/.aws/credentials`
- Create ProviderConfig referencing the secret
- Verify credentials have correct permissions
- Check credentials haven't expired

### Issue: Cloud resource not created

**Symptom:**
```
$ kubectl get bucket
NAME        READY   SYNCED
my-bucket   False   False
```

**How to diagnose:**
```bash
# Check resource conditions
kubectl describe bucket my-bucket

# Check provider logs
kubectl logs -n crossplane-system deployment/<provider-name>

# Check if credential has permissions
aws s3 ls  # Test with credentials directly
```

**Common causes:**
- Credentials lack required permissions
- Resource configuration is invalid for cloud provider
- Cloud provider API is down
- Quota exceeded (e.g., max buckets)

**Fix:**
- Verify IAM permissions for credentials
- Check resource configuration against cloud provider docs
- Test with cloud CLI: `aws s3 mb s3://test-bucket`
- Check cloud provider quotas and limits

---

## Operations Issues

### Issue: Operations feature not available

**Symptom:**
```
$ kubectl apply -f operation.yaml
error: unable to recognize "operation.yaml": no matches for kind "Operation" in version "ops.crossplane.io/v1alpha1"
```

**How to diagnose:**
```bash
# Check if operations CRD is installed
kubectl get crd | grep operations
```

**Common causes:**
- Operations not enabled in Crossplane installation
- Requires `--enable-operations` flag

**Fix:**
- Reinstall Crossplane with flag:
```bash
helm upgrade --install crossplane crossplane-stable/crossplane \
  --namespace crossplane-system \
  --set args='{"--enable-operations"}'
```

### Issue: Operation not running

**Symptom:**
```
$ kubectl describe operation check-cert
Status: <no output>
```

**How to diagnose:**
```bash
# Check operation is created
kubectl get operation

# Check operation status
kubectl describe operation <name>

# Check function is installed
kubectl get functions

# Check function is ready
kubectl describe function <function-name>
```

**Common causes:**
- Function not installed
- Operation function reference incorrect
- RBAC permissions missing

**Fix:**
- Install function: `kubectl apply -f function.yaml`
- Verify function name in operation matches installed function
- Check RBAC: `kubectl auth can-i get operations --as=system:serviceaccount:crossplane-system:crossplane`

---

## Debugging Strategies

### 1. Check Crossplane Control Plane Health

```bash
# Are all Crossplane pods running?
kubectl get pods -n crossplane-system

# Are there any errors?
kubectl logs -n crossplane-system deployment/crossplane

# Are all CRDs installed?
kubectl get crd | grep crossplane
```

### 2. Examine Resource Status

```bash
# Get detailed status
kubectl describe <resource-kind> <resource-name>

# Get YAML to see full spec/status
kubectl get <resource-kind> <resource-name> -o yaml

# Watch status changes
kubectl get <resource-kind> <resource-name> --watch
```

### 3. Check Conditions

Most Crossplane resources use conditions to report status:

```bash
kubectl get composite -o jsonpath='{.items[*].status.conditions}'
```

Common conditions:
- **Ready**: Resource is fully provisioned
- **Synced**: Resource matches desired state
- **Creating**: Resource is being created
- **Updating**: Resource is being updated

### 4. Look at Event Logs

```bash
# Events for resource
kubectl describe <resource-kind> <resource-name>

# Recent cluster events
kubectl get events -n crossplane-system --sort-by='.lastTimestamp'
```

### 5. Enable Debug Logging

```bash
# Reinstall with debug logging
helm upgrade crossplane crossplane-stable/crossplane \
  --namespace crossplane-system \
  --set args='{"--debug"}'

# Then check logs
kubectl logs -n crossplane-system deployment/crossplane
```

---

## Getting Help

Still stuck? Here's how to get help:

1. **Check the documentation:**
   - [Composition Guide]({{<ref "../composition">}})
   - [Managed Resources Guide]({{<ref "../managed-resources">}})
   - [Operations Guide]({{<ref "../operations">}})

2. **Search existing issues:**
   - [GitHub Issues](https://github.com/crossplane/crossplane/issues)

3. **Ask the community:**
   - [Slack Community](https://slack.crossplane.io)
   - [Community Forum](https://github.com/crossplane/crossplane/discussions)

4. **Report a bug:**
   - [File an issue on GitHub](https://github.com/crossplane/crossplane/issues/new)
   - Include logs, YAML files, and steps to reproduce

---

## Tips for Prevention

1. **Always start small**
   - Test with simple examples first
   - Add complexity incrementally

2. **Check prerequisites**
   - Kubernetes version compatible
   - Required permissions
   - Enough cluster resources

3. **Validate YAML**
   - Use `kubectl apply --dry-run=client`
   - Check schema with `kubectl explain`

4. **Monitor status**
   - Use `--watch` flag to see changes
   - Check logs frequently
   - Set up alerting for stuck resources

5. **Keep logs accessible**
   - Save logs to file for analysis
   - Review logs when things fail
   - Look for patterns in errors
