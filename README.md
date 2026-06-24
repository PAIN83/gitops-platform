# GitOps Platform (Version 2)

A modern cloud-native DevOps project that deploys a Flask web application 
using Kubernetes, Helm, ArgoCD, CircleCI, Prometheus, and Grafana.

This is Version 2 of a two-part portfolio. Version 1 covers traditional 
DevOps using AWS EC2, Terraform, Ansible, Docker, GitHub Actions, and 
CloudWatch — https://github.com/PAIN83/devops-production-setup

---

## What This Project Does

Every time code is pushed to GitHub, CircleCI automatically builds a new 
Docker image and pushes it to Docker Hub. ArgoCD detects the change and 
automatically deploys it to the Kubernetes cluster. No manual commands needed.
Git is the single source of truth — whatever is in GitHub is what runs 
in the cluster.

---

## Architecture

Developer pushes code to GitHub
→ CircleCI builds and pushes Docker image to Docker Hub
→ ArgoCD detects change and syncs Helm chart to Kubernetes
→ Kubernetes pulls new image and updates the app
→ Prometheus scrapes metrics, Grafana displays dashboards

---

## Tools Used

- Kubernetes — runs the app as pods, restarts them if they crash
- Minikube — local Kubernetes cluster for development
- Helm — packages Kubernetes manifests into one deployable chart
- ArgoCD — watches GitHub and auto-deploys any change to the cluster
- CircleCI — builds and pushes Docker image on every push to master
- Prometheus — collects CPU, memory, and pod metrics from the cluster
- Grafana — displays metrics as visual dashboards
- Terraform — EKS cluster architecture on AWS (designed, not provisioned 
  due to free tier account restrictions)

---

## Project Structure

The repository is organized by phase. Each folder represents one layer of the platform.

- **.circleci/** — CircleCI pipeline config that triggers on every push to master
- **flask-app/** — Helm chart containing templates and values.yaml
- **k8s/** — Raw Kubernetes manifests used in Phase 1 before Helm was introduced
- **eks/** — Terraform code for the AWS EKS production architecture
- **app.py, Dockerfile, requirements.txt** — The Flask application and container definition

---

## Phases

- Phase 1 — Deployed Flask app to Minikube using raw Kubernetes manifests
- Phase 2 — Packaged manifests into a Helm chart with configurable values
- Phase 3 — Added ArgoCD for fully automated GitOps deployments
- Phase 4 — Added Prometheus and Grafana for cluster observability
- Phase 5 — Wrote Terraform code for AWS EKS production architecture
- Phase 6 — Added CircleCI to automate Docker image builds and pushes

---

## Running Locally

# Start the cluster
minikube start --driver=docker

# Deploy the app
helm install flask-app ./flask-app

# Get the app URL
minikube service flask-app-service --url

# Install monitoring
helm install monitoring prometheus-community/kube-prometheus-stack \
  --namespace monitoring --create-namespace

# Access Grafana at http://localhost:3000
kubectl --namespace monitoring port-forward svc/monitoring-grafana 3000:80
