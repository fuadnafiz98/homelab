# Not gitops scripts

Quickly learning random things, will migrate to gitops when done.

## Databases in Kubernetes

### Statefulsets

- `kubectl create ns sfsets`
- `kubectl get storageclass`
- ``

### Persistent Volumes

its like a cluster resource like a RAM or CPU.
its also like a external plugin to the cluster.

### Persistent Volume Claim

used to claim volume of the cluster

so its something like

```
pod ➡️ PVC ➡️ PV
```

### Storage Class

provistions PV dynamically

### Kubernetes Operator

### Basic Kubernetes PostgreSQL deployment

1. `kubectl apply -f postgresql-pvc.yaml`
1. `kubectl apply -f postgresql-configmap.yaml`
1. `kubectl apply -f postgresql-secret.yaml`
1. `kubectl apply -f postgresql-statefulset.yaml`
1. `kubectl apply -f postgresql-service.yaml`
1. `kubectl exec -it postgres-0 -- bash`
1. `kubectl port-forward service/postgres 5432:5432`

### Resources

- [https://www.youtube.com/watch?v=0swOh5C3OVM](https://www.youtube.com/watch?v=0swOh5C3OVM)
- [https://www.youtube.com/watch?v=pPQKAR1pA9U](https://www.youtube.com/watch?v=pPQKAR1pA9U)
- [https://www.youtube.com/watch?v=zj6r_EEhv6s](https://www.youtube.com/watch?v=zj6r_EEhv6s)
