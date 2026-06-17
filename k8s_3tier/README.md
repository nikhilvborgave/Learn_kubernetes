# 3-Tier Architecture on Kubernetes 🏭

The same production-grade 3-tier app from Docker Compose — rebuilt using native Kubernetes objects.

## Architecture

```
Internet
    │
    ▼
Ingress (nginx)              ← single entry point
    │
    ▼
Backend (Node.js)            ← Deployment, 2 replicas
    │
    ├──▶ PostgreSQL          ← StatefulSet + PVC
    │
    └──▶ Redis               ← Deployment
```

## What This Covers
- Secret — storing sensitive credentials
- ConfigMap — storing connection details
- Deployment — stateless backend and Redis
- StatefulSet — PostgreSQL with persistent storage
- PV + PVC — data persistence across pod restarts
- ClusterIP Services — internal communication by service name
- Ingress — single entry point with path routing

## Project Structure

```
k8s-3tier/
├── backend/
│   ├── secret.yaml        ← DB credentials
│   ├── configmap.yaml     ← DB_HOST, REDIS_HOST etc
│   ├── deployment.yaml    ← Node.js app, 2 replicas
│   └── service.yaml       ← ClusterIP :80
├── postgres/
│   ├── pv.yaml            ← hostPath volume (Minikube)
│   ├── pvc.yaml           ← 1Gi storage claim
│   ├── statefulset.yaml   ← PostgreSQL with persistent storage
│   └── service.yaml       ← ClusterIP :5432
├── redis/
│   ├── deployment.yaml    ← Redis cache
│   └── service.yaml       ← ClusterIP :6379
└── ingress/
    └── ingress.yaml       ← routes / to backend
```

## Deployment Order

```bash
# 1. Secrets and config first
kubectl apply -f backend/secret.yaml
kubectl apply -f backend/configmap.yaml

# 2. Storage
kubectl apply -f postgres/pv.yaml
kubectl apply -f postgres/pvc.yaml

# 3. Databases
kubectl apply -f postgres/statefulset.yaml
kubectl apply -f postgres/service.yaml

# 4. Cache
kubectl apply -f redis/deployment.yaml
kubectl apply -f redis/service.yaml

# 5. Backend
kubectl apply -f backend/deployment.yaml
kubectl apply -f backend/service.yaml

# 6. Ingress last
kubectl apply -f ingress/ingress.yaml
```

## API Routes

| Route | Description |
|---|---|
| `GET /` | App info |
| `GET /health` | Checks backend, PostgreSQL, Redis |
| `GET /db-test` | Writes to PostgreSQL |
| `GET /redis-test` | Writes to Redis |

```bash
# get minikube IP
minikube ip

curl http://<minikube-ip>/
curl http://<minikube-ip>/health
curl http://<minikube-ip>/db-test
curl http://<minikube-ip>/redis-test
```
