# Emartapp on Kubernetes 🛒

A full microservices e-commerce application deployed on Kubernetes.
Original codebase from [devopshydclub/emartapp](https://github.com/devopshydclub/emartapp).

## Architecture

```
Internet
    │
    ▼
Nginx (NodePort :30080)      ← reverse proxy, single entry point
    │
    ├──▶ Angular Client :4200
    │
    ├──▶ Node.js API :5000
    │        └──▶ MongoDB :27017
    │
    └──▶ Java API :9000
             └──▶ MySQL :3306
```

## Services

| Service | Type | Image | Description |
|---|---|---|---|
| nginx | Deployment | nginx:latest | Reverse proxy + API gateway |
| client | Deployment | emartapp-client | Angular frontend |
| api | Deployment | emartapp-nodeapi | Node.js REST API |
| webapi | Deployment | emartapp-javaapi | Java Spring Boot API |
| emongo | StatefulSet | mongo:4 | MongoDB for Node.js API |
| emartdb | StatefulSet | mysql:8.0.33 | MySQL for Java API |

## What This Covers
- Multi-service microservices deployment
- StatefulSet for both MongoDB and MySQL
- volumeClaimTemplates — automatic PVC per database pod
- ConfigMap as nginx config file mount using subPath
- Individual secret key reference — `secretKeyRef`
- Service naming to match hardcoded app config
- NodePort for public access on Minikube

## Project Structure

```
emartapp-k8s/
├── mongodb/
│   ├── statefulset.yaml   ← MongoDB with volumeClaimTemplates
│   └── service.yaml       ← named "emongo" to match app config
├── mysql/
│   ├── secret.yaml        ← MYSQL_ROOT_PASSWORD, MYSQL_DATABASE
│   ├── statefulset.yaml   ← MySQL with volumeClaimTemplates
│   └── service.yaml       ← ClusterIP :3306
├── nodeapi/
│   ├── deployment.yaml    ← connects to emongo:27017
│   └── service.yaml       ← ClusterIP :5000
├── javaapi/
│   ├── deployment.yaml    ← SPRING_DATASOURCE env vars
│   └── service.yaml       ← ClusterIP :9000
├── client/
│   ├── deployment.yaml    ← Angular static files served by nginx
│   └── service.yaml       ← ClusterIP :4200
└── nginx/
    ├── configmap.yaml     ← nginx routing config
    ├── deployment.yaml    ← mounts configmap as file
    └── service.yaml       ← NodePort :30080
```

## Deployment Order

```bash
# 1. Databases first
kubectl apply -f mongodb/
kubectl apply -f mysql/

# 2. APIs
kubectl apply -f nodeapi/
kubectl apply -f javaapi/

# 3. Frontend
kubectl apply -f client/

# 4. Nginx last
kubectl apply -f nginx/
```

## How to Access

```bash
minikube ip    # get IP

# open in browser or curl
http://<minikube-ip>:30080
```

## Docker Images Used

```
nikhilborgave/emartapp-client:latest
nikhilborgave/emartapp-nodeapi:latest
nikhilborgave/emartapp-javaapi:latest
nginx:latest
mongo:4
mysql:8.0.33
```
