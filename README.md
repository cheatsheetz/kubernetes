# Kubernetes Cheat Sheet

Essential kubectl commands and Kubernetes concepts for container orchestration.

---

## Table of Contents
- [Basic Commands](#basic-commands)
- [Pod Management](#pod-management)
- [Deployment Management](#deployment-management)
- [Service Management](#service-management)
- [ConfigMaps & Secrets](#configmaps--secrets)
- [Namespaces](#namespaces)
- [Troubleshooting](#troubleshooting)
- [YAML Examples](#yaml-examples)

---

## Basic Commands

| Command | Description | Example |
|---------|-------------|---------|
| `kubectl version` | Show client/server version | `kubectl version` |
| `kubectl cluster-info` | Display cluster info | `kubectl cluster-info` |
| `kubectl config current-context` | Show current context | `kubectl config current-context` |
| `kubectl config get-contexts` | List contexts | `kubectl config get-contexts` |
| `kubectl config use-context <context>` | Switch context | `kubectl config use-context prod` |

## Pod Management

| Command | Description | Example |
|---------|-------------|---------|
| `kubectl get pods` | List pods | `kubectl get pods` |
| `kubectl get pods -o wide` | Detailed pod info | `kubectl get pods -o wide` |
| `kubectl describe pod <pod>` | Pod details | `kubectl describe pod nginx-pod` |
| `kubectl logs <pod>` | View pod logs | `kubectl logs nginx-pod` |
| `kubectl logs -f <pod>` | Follow logs | `kubectl logs -f nginx-pod` |
| `kubectl exec -it <pod> -- /bin/bash` | Shell into pod | `kubectl exec -it nginx-pod -- /bin/bash` |
| `kubectl delete pod <pod>` | Delete pod | `kubectl delete pod nginx-pod` |
| `kubectl port-forward <pod> <local>:<remote>` | Port forward | `kubectl port-forward nginx-pod 8080:80` |

## Deployment Management

| Command | Description | Example |
|---------|-------------|---------|
| `kubectl get deployments` | List deployments | `kubectl get deployments` |
| `kubectl create deployment <name> --image=<image>` | Create deployment | `kubectl create deployment nginx --image=nginx` |
| `kubectl scale deployment <name> --replicas=<n>` | Scale deployment | `kubectl scale deployment nginx --replicas=3` |
| `kubectl set image deployment/<name> <container>=<image>` | Update image | `kubectl set image deployment/nginx nginx=nginx:1.20` |
| `kubectl rollout status deployment/<name>` | Check rollout status | `kubectl rollout status deployment/nginx` |
| `kubectl rollout history deployment/<name>` | Rollout history | `kubectl rollout history deployment/nginx` |
| `kubectl rollout undo deployment/<name>` | Rollback deployment | `kubectl rollout undo deployment/nginx` |
| `kubectl delete deployment <name>` | Delete deployment | `kubectl delete deployment nginx` |

## Service Management

| Command | Description | Example |
|---------|-------------|---------|
| `kubectl get services` | List services | `kubectl get services` |
| `kubectl expose deployment <name> --type=<type> --port=<port>` | Create service | `kubectl expose deployment nginx --type=ClusterIP --port=80` |
| `kubectl describe service <service>` | Service details | `kubectl describe service nginx` |
| `kubectl delete service <service>` | Delete service | `kubectl delete service nginx` |

### Service Types
- `ClusterIP` - Internal cluster access only
- `NodePort` - External access via node IP:port
- `LoadBalancer` - Cloud provider load balancer
- `ExternalName` - DNS CNAME record

## ConfigMaps & Secrets

| Command | Description | Example |
|---------|-------------|---------|
| `kubectl create configmap <name> --from-literal=<key>=<value>` | Create ConfigMap | `kubectl create configmap app-config --from-literal=env=prod` |
| `kubectl create configmap <name> --from-file=<file>` | ConfigMap from file | `kubectl create configmap app-config --from-file=config.yaml` |
| `kubectl get configmaps` | List ConfigMaps | `kubectl get configmaps` |
| `kubectl create secret generic <name> --from-literal=<key>=<value>` | Create Secret | `kubectl create secret generic db-secret --from-literal=password=secret123` |
| `kubectl get secrets` | List Secrets | `kubectl get secrets` |

## Namespaces

| Command | Description | Example |
|---------|-------------|---------|
| `kubectl get namespaces` | List namespaces | `kubectl get namespaces` |
| `kubectl create namespace <name>` | Create namespace | `kubectl create namespace dev` |
| `kubectl config set-context --current --namespace=<name>` | Set default namespace | `kubectl config set-context --current --namespace=dev` |
| `kubectl get pods -n <namespace>` | List pods in namespace | `kubectl get pods -n kube-system` |
| `kubectl delete namespace <name>` | Delete namespace | `kubectl delete namespace dev` |

## Troubleshooting

| Command | Description | Example |
|---------|-------------|---------|
| `kubectl get events` | Cluster events | `kubectl get events --sort-by=.metadata.creationTimestamp` |
| `kubectl top nodes` | Node resource usage | `kubectl top nodes` |
| `kubectl top pods` | Pod resource usage | `kubectl top pods` |
| `kubectl describe node <node>` | Node details | `kubectl describe node worker-1` |
| `kubectl get pods --field-selector=status.phase=Failed` | Failed pods | `kubectl get pods --field-selector=status.phase=Failed` |

## YAML Examples

### Basic Pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx:1.20
    ports:
    - containerPort: 80
```

### Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.20
        ports:
        - containerPort: 80
```

### Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
```

### ConfigMap
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  database_url: "postgres://localhost:5432/mydb"
  debug: "true"
```

### Secret
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  username: dXNlcg==  # base64 encoded
  password: cGFzcw==  # base64 encoded
```

## Advanced Operations

### Labels and Selectors
- `kubectl get pods --selector=app=nginx` - Get pods with label
- `kubectl label pod <pod> env=production` - Add label to pod
- `kubectl label pod <pod> env-` - Remove label from pod

### Resource Management
- `kubectl apply -f <file>` - Apply configuration from file
- `kubectl delete -f <file>` - Delete resources from file
- `kubectl get all` - Get all resources
- `kubectl api-resources` - List available resource types

---

## Resources
- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
- [Kubernetes API Reference](https://kubernetes.io/docs/reference/)

---
*Originally compiled from various sources. Contributions welcome!*