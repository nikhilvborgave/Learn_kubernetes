# Kubernetes Learning Journey ☸️

A hands-on Kubernetes learning path from fundamentals to production-grade deployments.
Built while learning K8s concepts from scratch — each phase introduces new concepts with real hands-on practice.

## Learning Path

| Phase | Topic | Status |
|---|---|---|
| Phase 1 | Fundamentals — Pods, Namespaces, kubectl | ✅ |
| Phase 2 | Workloads — Deployment, ReplicaSet, DaemonSet, StatefulSet | ✅ |
| Phase 3 | Networking — ClusterIP, NodePort, LoadBalancer, Ingress | ✅ |
| Phase 4 | Configuration — ConfigMap, Secret | ✅ |
| Phase 5 | Storage — PV, PVC | ✅ |
| Phase 6 | Access Control — RBAC, ServiceAccount | ✅ |
| Phase 7 | Advanced — HPA, Network Policies, Helm | ✅ |
| Phase 8 | Projects | ✅ |

## Projects

| Project | Description |
|---|---|
| [K8s Basics](./k8s-basics/) | Hands-on YAML files from learning phases 1-7 |
| [Helm Project](./helm-project/) | Multi-environment Node.js API using custom Helm chart |
| [3-Tier Architecture](./k8s-3tier/) | Node.js + PostgreSQL + Redis on Kubernetes |
| [Emartapp](./emartapp-k8s/) | Full microservices app — Angular + Node.js + Java + MongoDB + MySQL |

## Concepts Covered

**Workloads**
- Deployment — rolling updates, rollback, scaling
- ReplicaSet — pod count management
- DaemonSet — one pod per node (log collectors, monitoring agents)
- StatefulSet — stable identity, ordered startup, persistent storage per pod

**Networking**
- ClusterIP — internal only
- NodePort — external via node IP
- LoadBalancer — cloud ELB provisioned automatically
- Ingress — path-based routing, one ELB for all services

**Configuration**
- ConfigMap — non-sensitive config
- Secret — sensitive data, base64 encoded

**Storage**
- PV — actual storage
- PVC — storage request
- Dynamic provisioning via StorageClass

**Access Control**
- Role + RoleBinding — namespace scoped
- ClusterRole + ClusterRoleBinding — cluster wide
- ServiceAccount — pod identity

**Advanced**
- HPA — auto scaling based on CPU/memory
- Network Policies — pod to pod traffic control
- Helm — package manager, multi-environment deployments

## Tools Used

```
minikube    ← local K8s cluster for learning
kubectl     ← K8s CLI
helm        ← package manager
```

## Author
**Nikhil** — learning Kubernetes from scratch.
