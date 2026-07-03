# GitOps

ArgoCD App-of-Apps repository for GitOps deployments across environments.

## Structure

```
gitops/
├── apps/
│   ├── root/                    # Root Application per environment
│   ├── platform/                # Platform components
│   ├── retail-store/            # Retail store microservices
│   └── overlays/                # Environment-specific value overlays
├── clusters/                    # Cluster configurations
├── rbac/                        # RBAC policies
├── notifications/               # Notification templates
└── README.md
```

## Usage

### Apply Root Application

```bash
kubectl apply -f apps/root/dev-root-application.yaml
```

### Sync Policies

- **Dev**: Auto-sync enabled
- **Stage**: Manual sync with health checks
- **Prod**: Manual sync with approval gates

## RBAC

- `team-dev`: Can sync dev environment only
- `team-ops`: Can sync stage/prod environments

## Notifications

Slack/webhook notifications on:
- Sync success/failure
- Manual sync required
- Health status changes
