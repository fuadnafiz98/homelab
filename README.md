# Homelab

My attempt to create a homelab. This repo will contain various configuration files for my homelab.

## Ansible

First install the ansible in the host machine only.

Then create a ssh key on the host machine and copy it to the remote machines / nodes / worker nodes.

Remember to add ssh key in the `~/.ssh/authorized_keys` and give it proper permissions.

In order to use `pacman` for arch linux, install `ansible-galaxy collection install community.general`

Then run the playbook: `ansible-playbook -i inventory/inventory.ini playbooks/update_system.yaml --ask-become-pass`

Here:

- `-i inventory/inventory.ini` will specify which inventory to use
- `--ask-become-pass` will ask for the root password for the remote machines

## Kubernetes

- Enable kubelet: `sudo systemctl enable --now kubelet`

- ` sudo kubeadm init --cri-socket=/run/containerd/containerd.sock`

- Install Flannel:

`kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml`

### Monitoring

we need node exporter for getting the stats. A good way for doing it is using `daemonSet` of K8s so it automatically installs `node-exporter` in every pod we will initiate.

We will use `hlem` as package manager. We will use this stack for promethus installation https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack

Initial installation:

```bash

 helm install prometheus prometheus-community/kube-prometheus-stack

```

Show Values:

```bash

helm show values prometheus-community/kube-prometheus-stack > values.yaml

```

Update:

```bash

helm install prometheus prometheus-community/kube-prometheus-stack -f values.yaml

```

## Rancher

The `rancher/` directory contains Kubernetes manifests and configuration for managing applications using the GitOps "app of apps" pattern, typically with Rancher Fleet or Argo CD. This approach allows you to declaratively manage and deploy multiple applications and their components in a scalable, modular way.

### Structure

- `app-of-apps.yaml`: The entry-point manifest that defines the top-level application, referencing other apps or components for deployment.
- `apps/`: Contains application definitions and components.
  - `applications.yaml`: Aggregates multiple application resources.
  - `components/`: Houses reusable components, such as the `nginx` example below.
    - `nginx/`: Example component with base and overlay configurations.
      - `base/`: Contains the base deployment, service, and kustomization files for nginx.
      - `overlays/`: Contains environment-specific overlays (e.g., `local/`).

### Usage

- Use the manifests in this folder to bootstrap and manage your Kubernetes workloads via Rancher Fleet, Argo CD, or similar GitOps tools.
- The "app of apps" pattern enables you to manage complex deployments by composing smaller, reusable application manifests.
- To deploy, apply the `app-of-apps.yaml` or use your GitOps tool to sync the repository.

For more details, see the `rancher/README.md` file.



