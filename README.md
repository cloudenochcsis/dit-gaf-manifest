# Ghana Armed Forces Directorate of IT - Kubernetes Manifests

## Overview

This repository contains the Kubernetes manifests for the official Ghana Armed Forces (GAF) Directorate of Information Technology (DIT) web application. The deployment is managed via a GitOps workflow, where this repository serves as the single source of truth for the application's desired state in the Kubernetes cluster.

## Manifests

This repository includes the following core Kubernetes manifests:

*   `deployment.yaml`: Defines the deployment of the `dit-gaf-web` application. It specifies the Docker image to be used, the number of replicas, and the health check probes (`livenessProbe` and `readinessProbe`) to ensure the application is running correctly.

*   `service.yaml`: Exposes the application pods via a Kubernetes Service. It is configured as type `LoadBalancer`, which provisions an external load balancer in a cloud environment to make the application accessible via a public IP address.

## GitOps Workflow

This repository is a critical component of an automated CI/CD pipeline:

1.  **Source Code Repository**: A separate repository holds the Vue.js application source code.
2.  **Continuous Integration (CI)**: When changes are pushed to the source code repository, a CircleCI pipeline automatically builds a new Docker image, pushes it to Docker Hub, and tags it with a unique build number.
3.  **Automated Update**: The CI pipeline then checks out this manifest repository, updates the `image` tag in `deployment.yaml` to the new build number, and commits the change back to this repository.
4.  **Continuous Delivery (CD)**: An Argo CD instance running in the Kubernetes cluster monitors this repository. Upon detecting the change, Argo CD automatically syncs the state of the cluster to match the updated manifests, deploying the new version of the application.

## Configuration

**Important**: The `image` tag in `deployment.yaml` is updated automatically by the CI/CD pipeline. Manual modifications to this field will be overwritten. All other configuration changes should be made via pull requests to this repository.
