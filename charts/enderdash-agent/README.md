# EnderDash Agent

The `enderdash-agent` chart installs the EnderDash standalone agent in a
Kubernetes cluster. The agent connects the cluster to an EnderDash server record
and exposes inventory, logs, diagnostics, resource views, and optional operator
actions.

## Prerequisites

- Kubernetes 1.24 or newer.
- Helm 3.
- An EnderDash agent key from the server setup page.

## Install

Create the namespace and Secret that stores the agent key:

```bash
kubectl create namespace enderdash
kubectl -n enderdash create secret generic enderdash-agent \
  --from-literal=agentKey='<agentKey>'
```

Install the chart:

```bash
helm repo add enderdash https://charts.enderdash.com
helm repo update
helm upgrade --install enderdash-agent enderdash/enderdash-agent \
  --namespace enderdash \
  --create-namespace \
  --set agentKeySecret.name=enderdash-agent \
  --set agentKeySecret.key=agentKey \
  --set rbac.mode=readonly
```

Use `rbac.mode=operator` only for clusters where EnderDash should run
Kubernetes mutations such as restarts, scaling, exec, port-forward, debug
containers, and YAML apply or delete actions.

## Values

| Value | Default | Description |
| --- | --- | --- |
| `image.repository` | `ghcr.io/enderdash-com/enderdash-agent` | Agent image repository. |
| `image.tag` | `latest` | Agent image tag. |
| `image.pullPolicy` | `Always` | Kubernetes image pull policy. |
| `imagePullSecrets` | `[]` | Image pull secrets for private registries. |
| `agentKeySecret.name` | `enderdash-agent` | Secret containing the EnderDash agent key. |
| `agentKeySecret.key` | `agentKey` | Secret key that stores the agent key value. |
| `rbac.mode` | `readonly` | `readonly` grants inventory and diagnostics access. `operator` adds Kubernetes mutation permissions. |
| `podAnnotations` | `{}` | Extra annotations for the agent Pod. |
| `podLabels` | `{}` | Extra labels for the agent Pod. |
| `podSecurityContext` | See `values.yaml` | Pod security context. |
| `securityContext` | See `values.yaml` | Container security context. |
| `resources` | See `values.yaml` | Container resource requests and limits. |
| `persistence.enabled` | `false` | Use a persistent volume for `/enderdash/data`. |
| `persistence.existingClaim` | `""` | Existing PVC name. |
| `persistence.storageClassName` | `""` | Storage class for a generated PVC. |
| `persistence.accessModes` | `["ReadWriteOnce"]` | Access modes for a generated PVC. |
| `persistence.size` | `1Gi` | Requested storage for a generated PVC. |
| `nodeSelector` | `{}` | Node selector for the agent Pod. |
| `tolerations` | `[]` | Tolerations for the agent Pod. |
| `affinity` | `{}` | Affinity rules for the agent Pod. |
