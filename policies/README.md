# Admission Policies

This directory contains Kyverno admission policies for enforcing security and best practices across the cluster.

## Policies

### Security Policies

- **disallow-latest-tag**: Prevents use of mutable `:latest` image tags
- **restrict-privileged-pods**: Prevents containers from running in privileged mode
- **restrict-host-path**: Restricts use of hostPath volumes

### Best Practice Policies

- **enforce-resource-limits**: Requires resource requests and limits for all containers
- **require-readiness-probe**: Requires readiness probes for all containers
- **require-liveness-probe**: Requires liveness probes for all containers

## Installation

### Install Kyverno

```bash
kubectl create -f https://github.com/kyverno/kyverno/releases/download/v1.10.0/install.yaml
```

### Apply Policies

```bash
cd /Users/devesh/workspace/assignment/gitops/policies
kubectl apply -f .
```

## Policy Enforcement

All policies are set to `validationFailureAction: enforce`, which means they will block non-compliant resources from being created or modified.

## Testing Policies

### Test a Policy Violation

```bash
# Try to create a pod with :latest tag (should be blocked)
kubectl run test-pod --image=nginx:latest
```

### View Policy Reports

```bash
kubectl get policyreport -A
kubectl describe policyreport <report-name>
```

## Policy Exceptions

To create an exception for a specific resource, use a Kyverno PolicyException:

```yaml
apiVersion: kyverno.io/v2
kind: PolicyException
metadata:
  name: exception-example
spec:
  match:
    any:
      - resources:
          kinds:
            - Pod
          names:
            - special-pod
  exceptions:
    - policyName: disallow-latest-tag
      ruleNames:
        - validate-image-tag
```

## Monitoring

Monitor policy violations using:

```bash
kubectl get policyreport -A -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.summary.errorCount}{"\n"}{end}'
```
