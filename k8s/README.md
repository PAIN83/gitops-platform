# Raw Kubernetes Manifests (Phase 1)

This folder contains the original Kubernetes manifests written before Helm 
was introduced to the project. They represent the first working deployment 
of the Flask app on Kubernetes.

## Why These Exist

These files are kept as a reference to show the starting point of the 
project — plain Kubernetes manifests without templating or variables. 
Phase 2 replaced this approach with a Helm chart (see the flask-app folder) 
which packages the same resources with configurable values.

## Files

- `deployment.yaml` — runs 2 replicas of the Flask app and restarts them 
  automatically if they crash
- `service.yaml` — exposes the app using a NodePort service, routes 
  traffic from port 80 to port 5000 inside the container

## Note

These manifests are not actively used in the current GitOps workflow. 
ArgoCD watches the Helm chart in the flask-app folder, not this folder.

## Manual Commands (for reference only)

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
