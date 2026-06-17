# ArgoCD OutOfSync Production Incident

## Objective

Simulate and troubleshoot an ArgoCD OutOfSync incident caused by manual Kubernetes changes.

---

## Architecture

GitHub
 |
 |
ArgoCD
 |
 |
Kind Kubernetes Cluster


Application:
payment-service

---

## Initial State

Deployment YAML in Git:

replicas: 3


ArgoCD Status:

Synced
Healthy


---

## Incident Simulation

A manual production change was performed:

kubectl scale deployment payment-service --replicas=5


Live Cluster:

replicas: 5


Git:

replicas: 3


---

## Detection

ArgoCD detected configuration drift:

Status:
OutOfSync

Health:
Healthy


---

## Investigation

Command used:

argocd app diff payment-service


Difference found:

Git:

replicas: 3


Cluster:

replicas: 5


---

## Root Cause

Manual modification was performed directly on Kubernetes instead of through Git.


---

## Resolution

Synced application back with Git:

argocd app sync payment-service


---

## Prevention

- Follow GitOps workflow
- Restrict direct kubectl access in production
- Enable ArgoCD selfHeal
- Use Kubernetes audit logs to track changes
