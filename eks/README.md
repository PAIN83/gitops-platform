# EKS Architecture (Not Provisioned)

This folder contains Terraform code written to provision a production AWS EKS cluster for this project.

## Why It Wasn't Provisioned

EKS worker nodes need at least a t3.medium instance to run properly. The AWS account used here is restricted to free tier instances only, so AWS blocked the node group from being created. To avoid unnecessary charges, terraform destroy was run to clean up everything that was partially created. The code is kept here to show the architecture was designed and tested up to that point.

## What This Code Creates

A VPC with public and private subnets across two availability zones, a NAT Gateway for internet access from private subnets, an EKS control plane, and a managed node group of t3.medium instances.

## How This Fits Into the Full Project

In production this would replace Minikube. The same Helm chart and ArgoCD setup used locally would deploy to this EKS cluster instead, with CircleCI building images and Prometheus/Grafana monitoring it the same way.

## Files

- `main.tf` — VPC and EKS module configuration
- `outputs.tf` — cluster name, endpoint, and region
