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
  --namespace enderdash
```

To check the deployment:

```bash
helm list -n enderdash
kubectl -n enderdash get pods -l app.kubernetes.io/name=enderdash-agent
```

The default install grants EnderDash cluster-wide Kubernetes permissions so it
can manage workloads, exec sessions, port-forwards, debug containers, and YAML
apply or delete operations.

Use `--set rbac.mode=readonly` when you want to restrict EnderDash to inventory
and log access.

## Values

| Value | Default | Description |
| --- | --- | --- |
| `image.repository` | `ghcr.io/enderdash-com/enderdash-agent` | Agent image repository. |
| `image.tag` | `latest` | Agent image tag. |
| `image.pullPolicy` | `Always` | Kubernetes image pull policy. |
| `imagePullSecrets` | `[]` | Image pull secrets for private registries. |
| `agentKeySecret.name` | `enderdash-agent` | Secret containing the EnderDash agent key. |
| `agentKeySecret.key` | `agentKey` | Secret key that stores the agent key value. |
| `rbac.mode` | `cluster-admin` | `cluster-admin` grants full cluster access. `readonly` restricts EnderDash to inventory and log access. |
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
