# Flask App — Helm Chart

This folder is a Helm chart that packages the Flask application's Kubernetes 
manifests into one deployable unit.

## Why Helm Instead of Raw Manifests

Managing separate deployment.yaml and service.yaml files gets messy once you 
need different settings for different environments. Helm solves this by 
templating the manifests and pulling actual values from a single values.yaml 
file. Changing replica count, image tag, or port only requires editing 
values.yaml — the templates never change.

## Files

- `Chart.yaml` — the identity of the chart, contains name and version
- `values.yaml` — all configurable values: app name, replica count, 
  image repository and tag, service port and type
- `templates/deployment.yaml` — Deployment template, references values 
  using `{{ .Values.x }}` syntax
- `templates/service.yaml` — Service template, exposes the app and routes 
  traffic to the pods

## How It's Used

This chart is what ArgoCD watches and syncs automatically. When values.yaml 
changes and is pushed to GitHub, ArgoCD detects it and runs the equivalent 
of helm upgrade to update the cluster — no manual commands needed.

## Manual Commands (for local testing)

```bash
# Install the chart
helm install flask-app ./flask-app

# Update the chart after changing values.yaml
helm upgrade flask-app ./flask-app

# Remove the chart
helm uninstall flask-app
