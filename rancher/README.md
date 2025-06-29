# Rancher + GitOps + K8s

---

This folder contains my experiment with rancher desktop with k8s installed. I will use gitops to deploy and manage
all the Pods and k8s infra.

## Commands

```sh

kubectl cluster-info
kubectl get pods -n default

# create argocd namespace:
kubectl create namespace argocd

```

it should show something like this:

» kubectl get namespaces
NAME STATUS AGE
argocd Active 43s
default Active 26h
kube-node-lease Active 26h
kube-public Active 26h
kube-system Active 26h

Install Manifest:
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

See the running services:
» kubectl get services -n argocd
NAME TYPE CLUSTER-IP EXTERNAL-IP PORT(S) AGE
argocd-applicationset-controller ClusterIP 10.43.123.180 <none> 7000/TCP,8080/TCP 2m2s
argocd-dex-server ClusterIP 10.43.41.98 <none> 5556/TCP,5557/TCP,5558/TCP 2m2s
argocd-metrics ClusterIP 10.43.41.232 <none> 8082/TCP 2m2s
argocd-notifications-controller-metrics ClusterIP 10.43.148.247 <none> 9001/TCP 2m2s
argocd-redis ClusterIP 10.43.56.23 <none> 6379/TCP 2m2s
argocd-repo-server ClusterIP 10.43.199.155 <none> 8081/TCP,8084/TCP 2m2s
argocd-server ClusterIP 10.43.183.61 <none> 80/TCP,443/TCP 2m2s
argocd-server-metrics ClusterIP 10.43.26.11 <none> 8083/TCP 2m2s

Port forward to access the UI:
kubectl port-forward svc/argocd-server -n argocd 8080:443

Username is `admin`
Password can be retrived by:
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
