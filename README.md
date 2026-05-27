# EnderDash Helm charts

This repository publishes the public Helm chart repository for EnderDash.
Chart source lives in `main` under `charts/`, while packaged chart releases and
the Helm `index.yaml` are published by GitHub Actions.

## Add the chart repository

```bash
helm repo add enderdash https://charts.enderdash.com
helm repo update
```

## Install the agent

Create the namespace and agent key Secret first:

```bash
kubectl create namespace enderdash
kubectl -n enderdash create secret generic enderdash-agent \
  --from-literal=agentKey='<agentKey>'
```

Install the read-only agent chart:

```bash
helm upgrade --install enderdash-agent enderdash/enderdash-agent \
  --namespace enderdash \
  --create-namespace \
  --set agentKeySecret.name=enderdash-agent \
  --set agentKeySecret.key=agentKey \
  --set rbac.mode=readonly
```

Use operator mode only for clusters where EnderDash should run Kubernetes
mutations such as restarts, scaling, exec, port-forward, debug containers, and
YAML apply or delete actions:

```bash
helm upgrade --install enderdash-agent enderdash/enderdash-agent \
  --namespace enderdash \
  --create-namespace \
  --set agentKeySecret.name=enderdash-agent \
  --set agentKeySecret.key=agentKey \
  --set rbac.mode=operator
```

## Releasing

Every chart release needs a new `version` in the chart's `Chart.yaml`.
Pushing chart changes to `main` runs chart-releaser, which packages changed
charts, creates GitHub releases, and updates the GitHub Pages index.
