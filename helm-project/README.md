# Helm Project — Multi-Environment Node.js API ⎈

A custom Helm chart that deploys a Node.js API to multiple environments (dev and prod) using a single chart with different values files.

## What This Covers
- Writing a Helm chart from scratch
- `values.yaml` — default dev values
- `values-prod.yaml` — prod overrides only
- Template syntax — `{{ .Values.* }}`, `{{ .Release.Name }}`, `if/end`, `with`
- Conditional HPA — only in prod
- Conditional resources — only in prod
- `helm install` vs `helm upgrade`
- `helm template` for dry run testing

## Project Structure

```
nodeapp/
├── Chart.yaml            ← chart metadata
├── values.yaml           ← dev defaults
├── values-prod.yaml      ← prod overrides
└── templates/
    ├── _helpers.tpl      ← reusable functions
    ├── deployment.yaml   ← see template
    ├── service.yaml      ← see template
    ├── configmap.yaml    ← see template
    └── hpa.yaml          ← only renders in prod
```

## Dev vs Prod Comparison

| Feature | Dev | Prod |
|---|---|---|
| Replicas | 1 | 3 |
| Service type | ClusterIP | LoadBalancer |
| Resources | none | cpu:200m, mem:256Mi |
| HPA | disabled | enabled (min:3 max:10 cpu:70%) |
| Log level | debug | info |
| Image tag | latest | v1.0.0 |

## How to Deploy

```bash
# dev
helm install nodeapp-dev ./nodeapp

# prod
helm install nodeapp-prod ./nodeapp -f values-prod.yaml

# dry run (test without deploying)
helm template nodeapp-dev ./nodeapp
helm template nodeapp-prod ./nodeapp -f values-prod.yaml

# upgrade
helm upgrade nodeapp-dev ./nodeapp

# rollback
helm rollback nodeapp-dev 1

# uninstall
helm uninstall nodeapp-dev
helm uninstall nodeapp-prod
```

## Key Helm Concepts Used

| Concept | Where used |
|---|---|
| `.Values.*` | replicas, image, service type, log level |
| `.Release.Name` | resource naming — prevents conflicts |
| `{{- if .Values.autoscaling.enabled }}` | HPA only in prod |
| `{{- with .Values.resources }}` | resources only if defined |
| Two values files | dev vs prod without duplicating chart |
