# EnderDash Agent

Installs the EnderDash standalone agent in a Kubernetes cluster.

## Before you begin

- Kubernetes 1.24 or newer
- Helm 3
- An agent key from the EnderDash server setup page

## Install

Create the namespace and Secret:

```bash
kubectl create namespace enderdash
kubectl -n enderdash create secret generic enderdash-agent \
  --from-literal=agentKey='<agentKey>'
```

Add the Helm repository and install the chart:

```bash
helm repo add enderdash https://charts.enderdash.com
helm repo update
helm install enderdash-agent enderdash/enderdash-agent \
  --namespace enderdash \
  --set rbac.mode=readonly
```

To check the deployment:

```bash
helm list -n enderdash
kubectl -n enderdash get pods -l app.kubernetes.io/name=enderdash-agent
```

Use `--set rbac.mode=operator` when EnderDash should be allowed to run
Kubernetes actions such as restarts, scaling, exec, port-forward, debug
containers, and YAML apply or delete.

## Values

| Value | Default | Description |
| --- | --- | --- |
| `image.repository` | `ghcr.io/enderdash-com/enderdash-agent` | Agent image repository. |
| `image.tag` | `latest` | Agent image tag. |
| `image.pullPolicy` | `Always` | Kubernetes image pull policy. |
| `imagePullSecrets` | `[]` | Image pull secrets for private registries. |
| `agentKeySecret.name` | `enderdash-agent` | Secret containing the EnderDash agent key. |
| `agentKeySecret.key` | `agentKey` | Secret key that stores the agent key value. |
| `rbac.mode` | `readonly` | `readonly` grants inventory access. `operator` adds Kubernetes action permissions. |
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
