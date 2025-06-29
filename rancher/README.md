# Rancher + GitOps + Kubernetes

---

## Introduction

This repository documents an experiment using Rancher Desktop with Kubernetes, managed via GitOps principles. The goal is to deploy and manage all pods and Kubernetes infrastructure declaratively using ArgoCD.

## Prerequisites

- Rancher Desktop installed with Kubernetes enabled
- `kubectl` CLI configured to access your cluster
- Internet access to fetch manifests

## Cluster Setup

Check your cluster status and running pods:

```sh
kubectl cluster-info
kubectl get pods -n default
```

## ArgoCD Installation

1. **Create the ArgoCD namespace:**

   ```sh
   kubectl create namespace argocd
   ```

2. **Install ArgoCD using the official manifest:**

   ```sh
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   ```

3. **Verify ArgoCD services are running:**

   ```sh
   kubectl get services -n argocd
   ```

   Example output:

   ```
   NAME                                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
   argocd-applicationset-controller     ClusterIP   10.43.123.180    <none>        7000/TCP,8080/TCP            2m2s
   argocd-dex-server                    ClusterIP   10.43.41.98      <none>        5556/TCP,5557/TCP,5558/TCP   2m2s
   argocd-metrics                       ClusterIP   10.43.41.232     <none>        8082/TCP                     2m2s
   argocd-notifications-controller-metrics ClusterIP 10.43.148.247   <none>        9001/TCP                     2m2s
   argocd-redis                         ClusterIP   10.43.56.23      <none>        6379/TCP                     2m2s
   argocd-repo-server                   ClusterIP   10.43.199.155    <none>        8081/TCP,8084/TCP            2m2s
   argocd-server                        ClusterIP   10.43.183.61     <none>        80/TCP,443/TCP               2m2s
   argocd-server-metrics                ClusterIP   10.43.26.11      <none>        8083/TCP                     2m2s
   ```

## Accessing the ArgoCD UI

To access the ArgoCD web UI, use port-forwarding:

```sh
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

- **Username:** `admin`
- **Password:** Retrieve the initial admin password with:

  ```sh
  kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
  ```

## Repository File Structure

```
homelab/rancher
├── apps/                                 # Parent application (App of Apps)
│   ├── kustomization.yaml                # Groups child Application resources for ArgoCD
│   └── applications.yaml                 # Defines child Application for Nginx
│   └── components/                       # Child application manifests
│       └── nginx/                        # Nginx application
│           ├── base/                     # Base manifests for Nginx, reusable across environments
│           │   ├── deployment.yaml       # Defines the Nginx Deployment
│           │   ├── service.yaml          # Defines the Nginx Service
│           │   └── kustomization.yaml    # Groups base manifests for Kustomize
│           └── overlays/                 # Environment-specific customizations
│               └── local/                # Local environment overlay
│                   ├── kustomization.yaml# Customizes base manifests for local environment
│                   └── patch-replicas.yaml # Patches the replicas field of the Nginx Deployment
├── app-of-apps.yaml                      # Defines the parent Application (App of Apps)
└── README.md                             # Documentation for the repository
```

## References

- [ArgoCD Official Documentation](https://argo-cd.readthedocs.io/en/stable/)
- [Kustomize Documentation](https://kubectl.docs.kubernetes.io/references/kustomize/)
- [Rancher Desktop](https://rancherdesktop.io/)

---

Feel free to contribute or open issues for improvements!
