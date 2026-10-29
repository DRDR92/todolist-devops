# ToDo App on Kubernetes

Django ToDo application deployed to a local multi-node Kubernetes cluster with Helm.

> The application code is based on an open-source Django ToDo app (MIT License) provided as part of the Mate Academy DevOps course. All infrastructure (Kubernetes manifests, Helm chart, cluster setup) is my own work.

## Tech stack

Docker · Kubernetes (kind) · Helm · MySQL · Ingress NGINX · Metrics Server · Bash

## Architecture

- **kind cluster:** 1 control-plane node and 7 worker nodes, labeled for scheduling
- **App:** Deployment with HPA (2–5 replicas, CPU/memory based), NodePort and ClusterIP services, Ingress
- **Database:** MySQL StatefulSet (2 replicas) as a Helm sub-chart, with persistent storage
- **Scheduling:** node affinity, pod anti-affinity, taints and tolerations keep MySQL on dedicated nodes
- **Security:** RBAC service account with read-only access to secrets; credentials kept out of the repository

## Run locally

Requirements: Docker, kind, kubectl, Helm.

Create a secrets file from the template and set your own values:

```bash
cp .infrastructure/helm-chart/todoapp/secrets.example.yaml .infrastructure/helm-chart/todoapp/secrets.yaml
```

Then run:

```bash
./bootstrap.sh
```

The script creates the cluster, installs Ingress NGINX and Metrics Server, and deploys the Helm chart.

The app is available at http://localhost:30007

## Project structure

```
src/                         Django app and Dockerfile
.infrastructure/helm-chart/  Helm chart (todoapp + mysql sub-chart)
.infrastructure/             Plain Kubernetes manifests (pre-Helm version)
cluster.yml                  kind cluster config
bootstrap.sh                 One-command setup
```

## Roadmap

- [ ] CI/CD with GitHub Actions: build and push my own Docker image
- [x] Move secrets out of values.yaml
- [ ] Monitoring with Prometheus and Grafana