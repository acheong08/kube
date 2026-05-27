# Node Labels

All custom labels use the `duti.dev/` domain prefix following Kubernetes
conventions.

## Label Reference

### `location.duti.dev/network`

Describes the network topology of the node.

| Value    | Meaning                              |
| -------- | ------------------------------------ |
| `home`   | Node is on the home/lan network      |
| `public` | Node is directly internet-accessible |

**Usage:** Schedule workloads that must (or must not) be exposed to the
internet.

```yaml
# Schedule on a public node
nodeSelector:
  location.duti.dev/network: public

# Avoid home network nodes
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
        - matchExpressions:
            - key: location.duti.dev/network
              operator: NotIn
              values:
                - 'home'
```

### `node-role.duti.dev/ingress`

Indicates the node handles incoming traffic (Traefik, etc.). Follows the same
convention as built-in `node-role.kubernetes.io/*` labels.

| Value  | Meaning                       |
| ------ | ----------------------------- |
| `true` | Node runs ingress controllers |

**Usage:**

```yaml
nodeSelector:
  node-role.duti.dev/ingress: 'true'
```

### `capability.duti.dev/performance`

Describes the compute capability of the node.

| Value  | Meaning                                                     |
| ------ | ----------------------------------------------------------- |
| `high` | Beefier hardware — suitable for CPU/RAM-intensive workloads |

Nodes without this label are standard performance.

**Usage:**

```yaml
nodeSelector:
  capability.duti.dev/performance: high
```

## Current Node Assignments

| Node          | `location.duti.dev/network` | `node-role.duti.dev/ingress` | `capability.duti.dev/performance` |
| ------------- | --------------------------- | ---------------------------- | --------------------------------- |
| dell          | home                        | true                         | high                              |
| kube.duti.dev | public                      | true                         | —                                 |
| luddite.dev   | public                      | —                            | high                              |
| mailcow       | public                      | —                            | —                                 |
| nuc           | home                        | —                            | high                              |
| rock-3c       | home                        | —                            | —                                 |

## Workloads Using Custom Labels

| Workload            | Label(s) Used                                                      | Method            |
| ------------------- | ------------------------------------------------------------------ | ----------------- |
| traefik             | `node-role.duti.dev/ingress=true`                                  | nodeSelector      |
| invidious-db        | `capability.duti.dev/performance=high`                             | nodeSelector      |
| invidious           | `capability.duti.dev/performance=high`, `kubernetes.io/arch=amd64` | nodeSelector      |
| invidious-companion | `capability.duti.dev/performance=high`                             | nodeSelector      |
| yt-proxy            | `capability.duti.dev/performance=high`                             | nodeSelector      |
| anubis              | `capability.duti.dev/performance=high`, `kubernetes.io/arch=amd64` | nodeSelector      |
| caddy (invidious)   | `capability.duti.dev/performance=high`                             | nodeSelector      |
| conduit             | `capability.duti.dev/performance=high`                             | nodeAffinity (In) |
