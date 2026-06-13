# Vaultwarden Helm Chart

A Helm chart for deploying [Vaultwarden](https://github.com/dani-garcia/vaultwarden) (the unofficial Bitwarden server) on Kubernetes.

## Prerequisites

- Kubernetes 1.19+
- Helm 3+

## Installation

```bash
helm install vaultwarden ./charts/vaultwarden
```

## Configuration

The following table lists configurable parameters of the Vaultwarden chart and their default values:

| Parameter | Description | Default |
|-----------|-------------|---------|
| `application.name` | Application name | `vaultwarden` |
| `image.repository` | Image repository | `vaultwarden/server` |
| `image.tag` | Image tag | `1.33.2` |
| `service.port` | Service port | `80` |
| `persistence.enabled` | Enable persistent storage | `true` |
| `persistence.size` | PVC size | `1Gi` |
| `persistence.mountPath` | Mount path for data | `/data` |
| `persistence.accessMode` | Access mode | `ReadWriteOnce` |
| `ingress.enabled` | Enable Ingress | `false` |
| `resources.limits.cpu` | CPU limit | `500m` |
| `resources.limits.memory` | Memory limit | `256Mi` |
| `resources.requests.cpu` | CPU request | `100m` |
| `resources.requests.memory` | Memory request | `128Mi` |
| `env` | Environment variables | `WEB_VAULT_ENABLED=true`, `SIGNUPS_ALLOWED=false`, `INVITATIONS_ALLOWED=false` |

### Important Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `ADMIN_TOKEN` | Admin interface password (highly recommended to set) | Unset (must be set for production) |
| `SIGNUPS_ALLOWED` | Allow new user registrations | `false` |
| `INVITATIONS_ALLOWED` | Allow invitation emails | `false` |
| `WEB_VAULT_ENABLED` | Enable the web vault | `true` |

## Usage

### Basic Installation

```bash
helm install vaultwarden ./charts/vaultwarden
```

### With Custom Values

```bash
helm install vaultwarden ./charts/vaultwarden -f custom-values.yaml
```

### Example: Enabling Ingress with TLS

```yaml
ingress:
  enabled: true
  className: nginx
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt-prod
  hosts:
    - host: vaultwarden.example.com
      paths:
        - path: /
          pathType: Prefix
  tls:
    - secretName: vaultwarden-tls
      hosts:
        - vaultwarden.example.com
```

### Example: Setting Admin Token

```yaml
env:
  - name: ADMIN_TOKEN
    valueFrom:
      secretKeyRef:
        name: vaultwarden-secrets
        key: admin-token
```

## Persistence

The `/data` directory stores all encrypted vault data including:
- User databases
- Attachments
- Icons cache
- RSA keys

**WARNING**: This directory contains all your sensitive vault data. Always backup the PVC before upgrading or deleting the chart.

## Security Recommendations

1. **Change the default admin token** - Always set a strong `ADMIN_TOKEN` in production
2. **Enable TLS** - Use HTTPS for all connections via ingress or LoadBalancer
3. **Restrict signups** - Set `SIGNUPS_ALLOWED=false` after creating admin accounts
4. **Use secrets** - Store sensitive environment variables in Kubernetes Secrets, not plain values

## License

This chart is provided as-is. See [Vaultwarden LICENSE](https://github.com/dani-garcia/vaultwarden/blob/master/LICENSE).