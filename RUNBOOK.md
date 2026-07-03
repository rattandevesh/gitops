# GitOps Runbook

This runbook provides step-by-step instructions for managing deployments with ArgoCD GitOps.

## Prerequisites

- ArgoCD installed
- kubectl configured
- Access to gitops repository
- Applications repository configured

## Initial Setup

### 1. Install ArgoCD

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### 2. Access ArgoCD UI

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Open browser at https://localhost:8080

Default password:
```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

### 3. Configure CLI

```bash
argocd login localhost:8080 --insecure
```

### 4. Register Cluster

```bash
argocd cluster add <context-name>
```

## Application Deployment

### Deploy Root Application (Dev)

```bash
cd /Users/devesh/workspace/assignment/gitops
kubectl apply -f apps/root/dev-root-application.yaml
```

### Deploy Root Application (Stage)

```bash
kubectl apply -f apps/root/stage-root-application.yaml
```

### Deploy Root Application (Prod)

```bash
kubectl apply -f apps/root/prod-root-application.yaml
```

### Sync Application

```bash
argocd app sync dev-root
```

### Sync with Options

```bash
argocd app sync dev-root --prune --force
```

## Day 2 Operations

### Manual Sync

```bash
argocd app sync <app-name>
```

### Sync Specific Resource

```bash
argocd app sync <app-name> --resource <kind>:<namespace>/<name>
```

### Refresh Application

```bash
argocd app get <app-name> --refresh
```

### Delete Application

```bash
argocd app delete <app-name>
```

## Promotion Workflow

### Dev to Stage Promotion

1. Update stage values in gitops repository
2. Commit and push changes
3. Manual sync to stage
4. Verify deployment

### Stage to Prod Promotion

1. Update prod values with image digest
2. Commit and push changes
3. Request approval
4. Manual sync to prod
5. Verify deployment

## Troubleshooting

### Sync Failed

```bash
argocd app get <app-name>
argocd app logs <app-name>
```

### Out of Sync

```bash
argocd app diff <app-name>
argocd app sync <app-name>
```

### Resource Not Found

```bash
kubectl get all -n <namespace>
argocd app resources <app-name>
```

## Monitoring

Monitor ArgoCD for:
- Sync status
- Health status
- Application drift
- Sync failures
