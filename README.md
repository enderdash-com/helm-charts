# EnderDash Helm charts

[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/enderdash)](https://artifacthub.io/packages/search?repo=enderdash)

Helm chart repository for EnderDash.

## Add the repository

```bash
helm repo add enderdash https://charts.enderdash.com
helm repo update
helm search repo enderdash
```

## Install the agent

Create the namespace and Secret:

```bash
kubectl create namespace enderdash
kubectl -n enderdash create secret generic enderdash-agent \
  --from-literal=agentKey='<agentKey>'
```

Install the chart:

```bash
helm install enderdash-agent enderdash/enderdash-agent \
  --namespace enderdash
```

To check the release:

```bash
helm list -n enderdash
kubectl -n enderdash get pods -l app.kubernetes.io/name=enderdash-agent
```

The default install grants EnderDash cluster-wide Kubernetes permissions so it
can manage workloads, exec sessions, port-forwards, debug containers, and YAML
apply or delete operations.

Use `--set rbac.mode=readonly` when you want to restrict EnderDash to inventory
and log access.

## Releasing

Bump `charts/enderdash-agent/Chart.yaml` before publishing a new chart version.
Pushing chart changes to `main` runs chart-releaser, creates a GitHub release,
and updates the GitHub Pages index.
