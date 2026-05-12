# Automated Kubernetes Platform with RKE2, Istio & GitOps

<img width="513" height="344" alt="image" src="https://github.com/user-attachments/assets/5fdfa15c-40fa-4966-90e7-c5325501c28e" />



# Automated Kubernetes Platform with RKE2, HAProxy, Istio & GitOps

## Overview

This project automates the deployment of a production-ready Kubernetes platform using:

* RKE2 Kubernetes Cluster
* HAProxy High Availability Load Balancer
* Istio Service Mesh
* MetalLB Load Balancer
* Argo CD GitOps
* Argo Rollouts Canary Deployments
* GitHub Actions with Self-Hosted Runner
* Prometheus & Grafana Monitoring
* Kiali & Jaeger Observability
* Automated Canary Analysis & Rollback

The entire platform is provisioned and bootstrapped using Ansible automation.

---

# Architecture Overview

## Core Components

| Component                 | Purpose                                        |
| ------------------------- | ---------------------------------------------- |
| RKE2                      | Lightweight & secure Kubernetes distribution   |
| HAProxy                   | High availability Kubernetes API load balancer |
| Ansible                   | Infrastructure automation & bootstrap          |
| GitHub Actions            | CI/CD workflow automation                      |
| Self Hosted GitHub Runner | Execute workflows inside infrastructure        |
| Istio                     | Service mesh & traffic management              |
| MetalLB                   | Bare-metal load balancer                       |
| Argo CD                   | GitOps continuous delivery                     |
| Argo Rollouts             | Progressive delivery & canary deployment       |
| Prometheus                | Metrics collection                             |
| Grafana                   | Visualization & dashboards                     |
| Kiali                     | Istio service mesh observability               |
| Jaeger                    | Distributed tracing                            |

---

# Features

## Infrastructure Automation

* Automated RKE2 control plane deployment
* Automated worker node provisioning
* HAProxy setup for Kubernetes API high availability
* Fully automated cluster bootstrap using Ansible
* Reusable Ansible roles and inventories

## High Availability Control Plane

* HAProxy configured in front of Kubernetes API servers
* Load balancing across RKE2 control plane nodes
* API server failover support
* Health checks for cluster resiliency
* Highly available Kubernetes API endpoint

## CI/CD Automation

* GitHub Actions workflow integration
* Self-hosted GitHub runner deployment
* Automated validation and deployment pipeline
* GitOps workflow integration with Argo CD
* Infrastructure-as-Code deployment automation

## Service Mesh

* Istio service mesh installation
* Ingress Gateway configuration
* Traffic routing and splitting
* mTLS-ready architecture
* Retry & circuit breaking support
* Advanced observability

## GitOps Deployment

* Argo CD installation using Ansible
* GitOps-based application deployment
* AppSet-based addon deployment
* Declarative Kubernetes manifests

## Observability Stack

Installed via Argo CD ApplicationSets:

* Prometheus
* Grafana
* Kiali
* Jaeger

## Canary Deployment

* Argo Rollouts integrated with Istio
* Traffic split between stable and canary versions
* Progressive delivery strategy
* Automated promotion or rollback

## Automated Rollback

Canary analysis is driven by Prometheus metrics:

* Error Rate
* Response Time
* Success Rate
* Throughput

Rollback is automatically triggered when thresholds are breached.

---

# Project Workflow

```text
GitHub Actions Workflow
            ↓
Self Hosted Runner Executes Pipeline
            ↓
Ansible Automation
            ↓
Configure HAProxy
            ↓
Provision RKE2 Cluster
            ↓
Install Istio Service Mesh
            ↓
Install MetalLB
            ↓
Install Argo CD
            ↓
Install Argo Rollouts
            ↓
Deploy Observability Stack
            ↓
Deploy Booking Application
            ↓
Istio Canary Deployment
            ↓
Prometheus Metrics Analysis
            ↓
Promote OR Auto Rollback
```

---

# Booking Application

The sample microservices application contains:

* Product Service
* Reviews Service
* Rating Service

## Canary Strategy

The `reviews` service is deployed using:

* Istio traffic routing
* Argo Rollouts canary strategy
* Prometheus AnalysisTemplate
* Automated rollback policies

---

# Repository Structure

```bash
repo/
└── ansible-playbook/
    ├── install.yml
    ├── inventory/
    ├── group_vars/
    ├── host_vars/
    ├── roles/
    │   ├── rke2/
    │   ├── haproxy/
    │   ├── istio/
    │   ├── metallb/
    │   ├── argocd/
    │   ├── argo-rollouts/
    │   ├── github-runner/
    │   └── monitoring/
    │
    ├── manifests/
    │   ├── istio/
    │   ├── argocd/
    │   ├── rollouts/
    │   ├── monitoring/
    │   └── booking-app/
    │
    └── github-actions/
        └── workflow.yml
```

---

# Deployment Steps

## 1. Clone Repository

```bash
git clone https://github.com/yourusername/your-repo.git
cd repo/ansible-playbook
```

---

## 2. Configure Inventory

Update inventory with:

* HAProxy nodes
* Control plane nodes
* Worker nodes
* SSH credentials

Example:

```ini
[haproxy]
lb1 ansible_host=10.0.0.5

[rke2_servers]
master1 ansible_host=10.0.0.10
master2 ansible_host=10.0.0.11
master3 ansible_host=10.0.0.12

[rke2_agents]
worker1 ansible_host=10.0.0.20
worker2 ansible_host=10.0.0.21
```

---

## 3. Deploy Platform

```bash
ansible-playbook install.yml
```

This installs:

* HAProxy
* RKE2
* Istio
* MetalLB
* Argo CD
* Argo Rollouts
* GitHub self-hosted runner
* Monitoring stack
* Booking application

---

# GitHub Actions Workflow

## CI/CD Flow

```text
Developer Pushes Code
        ↓
GitHub Actions Triggered
        ↓
Self Hosted Runner Executes Workflow
        ↓
Validate Kubernetes Manifests
        ↓
Deploy Infrastructure Changes
        ↓
Argo CD Syncs Applications
        ↓
Argo Rollouts Starts Canary Deployment
```

---

# Canary Deployment Example

## Argo Rollouts Strategy

```yaml
strategy:
  canary:
    canaryService: reviews-canary
    stableService: reviews-stable
    trafficRouting:
      istio:
        virtualService:
          name: reviews-vs
          routes:
            - primary
```

---

# Prometheus AnalysisTemplate Example

```yaml
apiVersion: argoproj.io/v1alpha1
kind: AnalysisTemplate
metadata:
  name: success-rate
spec:
  metrics:
  - name: success-rate
    interval: 30s
    successCondition: result[0] >= 95
```

---

# GitOps Workflow

```text
Git Commit
    ↓
GitHub Actions Workflow
    ↓
Self Hosted Runner
    ↓
Argo CD Detects Changes
    ↓
Sync Kubernetes Manifests
    ↓
Argo Rollouts Starts Canary
    ↓
Prometheus Validates Metrics
    ↓
Promote OR Rollback
```

---

# Monitoring & Observability

## Grafana

Visualize:

* Cluster metrics
* Node metrics
* Application metrics
* Istio telemetry

## Kiali

Observe:

* Service graph
* Traffic flow
* mTLS status
* Request health

## Jaeger

Trace:

* Service-to-service communication
* Request latency
* Distributed transactions

---

# Security Features

* Kubernetes RBAC
* Istio mTLS-ready setup
* Namespace isolation
* Declarative GitOps workflow
* Controlled rollout strategy
* HAProxy API isolation

---

# Tech Stack

* Kubernetes (RKE2)
* Ansible
* HAProxy
* GitHub Actions
* Self Hosted GitHub Runner
* Istio
* MetalLB
* Argo CD
* Argo Rollouts
* Prometheus
* Grafana
* Kiali
* Jaeger
* YAML
* GitOps

---

# Future Improvements

* HashiCorp Vault integration
* External DNS automation
* Cluster autoscaling
* Multi-cluster GitOps
* Kyverno policies
* Falco runtime security
* Loki centralized logging
* OpenTelemetry integration

---

# LinkedIn Project Summary

Built a fully automated production-grade Kubernetes platform using RKE2, HAProxy, Ansible, Istio, GitHub Actions, and Argo CD with GitOps-driven canary deployments.

## Key Highlights

* Automated RKE2 cluster provisioning
* HAProxy high availability Kubernetes API
* GitHub Actions with self-hosted runner
* Istio service mesh integration
* MetalLB load balancer setup
* GitOps delivery using Argo CD
* Progressive delivery using Argo Rollouts
* Canary analysis with Prometheus metrics
* Auto rollback on failure detection
* Full observability with Grafana, Kiali & Jaeger

This project demonstrates real-world Kubernetes platform engineering with automation, resiliency, observability, GitOps, and progressive delivery.

---

# Author

Naresh Kumar Murugan

DevOps Engineer | Platform Engineer | Kubernetes | GitOps | Service Mesh | Cloud Automation
