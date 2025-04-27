# simple-app Helm Chart

This is a reusable Helm chart for deploying simple web applications with optional persistence and ingress.

## Features
- Configurable image repository, tag, and pull policy
- Service with customizable port and type
- Optional Ingress with TLS support
- Optional PersistentVolumeClaim for data storage
- Security context and environment variables

## Values
See [values.yaml](values.yaml) for all default configurations. Key settings include:

- `image.repository`: Container image repository
- `image.tag`: Container image tag
- `service.port` and `service.targetPort`: Service port mappings
- `ingress.enabled`: Enable or disable Ingress
 - `ingress.auth.enabled`: Enable optional Caddy-based basic authentication proxy (requires `ingress.enabled=true`)
 - `ingress.auth.secretName`: Kubernetes Secret name containing the password hash under key `hash` (default: `password-hash`)
 - `ingress.auth.username`: Username for basic auth (default: `user`)
 - `ingress.auth.image.repository`: Caddy image repository (default: `caddy`)
 - `ingress.auth.image.tag`: Caddy image tag (default: `latest`)
 - `ingress.auth.servicePort`: Port exposed by the Caddy proxy (default: `80`)
- `persistence.enabled`: Enable or disable PVC
- `securityContext`: Pod security context
- `env`: Environment variables for container

## Usage
You can use this chart as a standalone chart or as a subchart in an umbrella chart.

Example to deploy calendar application:
```bash
helm install calendar ./simple-app \
  --namespace calendar --create-namespace \
  --set nameOverride=calendar \
  --set image.repository=ghcr.io/jelmer/xandikos \
  --set image.tag=latest \
  --set service.port=8000 \
  --set service.targetPort=8000 \
  --set persistence.enabled=true \
  --set persistence.existingClaim=calendar-pvc \
  --set ingress.enabled=true \
  --set ingress.hosts[0].host=calendar.example.com \
  --set ingress.hosts[0].path=/ \
  --set ingress.tls[0].hosts[0]=calendar.example.com \
  --set ingress.tls[0].secretName=calendar-tls
```

Replace values as needed for other applications (e.g., vaultwarden, webdav).