# CircleCI Pipeline

This folder contains the CircleCI configuration that automates building 
and pushing the Docker image whenever code is pushed to master.

## What It Does

Every push to the master branch triggers CircleCI to checkout the code, 
build the Flask Docker image, log in to Docker Hub, and push the updated 
image. This removes the need to manually run docker build and docker push.

## Why This Matters

Without this, ArgoCD has no way to know a new image is available — it only 
watches for changes in the Helm chart. CircleCI closes this gap by 
automatically rebuilding and republishing the image on every code change, 
completing the full CI/CD loop alongside ArgoCD.

## File

`config.yml` defines one job called build-and-push that runs on every push 
to master. It uses setup_remote_docker to run Docker commands inside the 
CircleCI environment, builds the image tagged as pain0383/flask-app:latest, 
and pushes it to Docker Hub using credentials stored as environment 
variables in CircleCI project settings (DOCKERHUB_USERNAME and 
DOCKERHUB_PASSWORD).

## Full CI/CD Flow

Push to GitHub → CircleCI builds and pushes Docker image → ArgoCD detects 
Helm chart change → ArgoCD syncs cluster → app updates automatically
