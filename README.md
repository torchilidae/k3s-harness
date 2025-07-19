# k3s-harness

This repository contains Kubernetes manifests and GitOps configurations for deploying and managing a homelab Kubernetes cluster using K3s, ArgoCD, and Harness GitOps.

---

## Overview

This repo organizes the Kubernetes resources for a homelab environment into modular domains:

- **media/** — Media-related services (Radarr, Sonarr, Jellyfin, etc.)  
- **monitor/** — Monitoring tools (Grafana, Kubernetes Dashboard, Uptime Kuma)  
- **network/** — Network infrastructure components (Traefik, MetalLB, cert-manager, external-dns)  
- **security/** — Security and authentication (Authentik)  
- **storage/** — Storage solutions (Longhorn)  
- **config/** — Centralized ArgoCD application manifests and cluster configuration  

Each domain contains:

- Kubernetes manifests or Helm charts for deploying individual services  
- An ArgoCD Application manifest (`argocd-*.yaml`) for syncing resources within the domain  

---

## Features

- **Single environment setup:** Simplified configuration tailored for a homelab with no multi-environment complexity  
- **Modular structure:** Easy to add, remove, or update individual services without affecting others  
- **GitOps-driven:** Uses ArgoCD to continuously reconcile desired state from Git to the cluster  
- **Integrated monitoring and networking:** Built-in Grafana dashboards, Kubernetes dashboard, and network ingress/load balancing  
- **Automated dependency updates:** Renovate bot configured for automatic dependency management  

---

## Prerequisites

- Kubernetes cluster running K3s (or compatible distribution)  
- ArgoCD installed and configured on the cluster  
- Harness GitOps (optional, for pipeline automation)  
- `kubectl` CLI configured for your cluster  

---

## Usage

### Deploying the entire stack

- Clone this repository:  
  ```bash
  git clone https://github.com/torchilidae/k3s-harness.git
  cd k3s-harness
  ```

- Apply ArgoCD application manifests from `config/argocd-main.yaml` to bootstrap ArgoCD apps:  
  ```bash
  kubectl apply -f config/argocd-main.yaml
  ```

- Login to ArgoCD UI, sync all applications to deploy services across domains.

### Managing Applications

- To add a new app or service:  
  - Create a new folder inside the appropriate domain (e.g., `media/myapp`)  
  - Add Kubernetes manifests or Helm charts for the service  
  - Update the corresponding `argocd-*.yaml` manifest to include the new app  
  - Commit and push changes to trigger ArgoCD sync  

- To update an app:  
  - Modify manifests inside the app folder  
  - Commit and push; ArgoCD will detect changes and sync  

---

## Secrets Management

This repo does **NOT** include plaintext secrets. Use sealed-secrets, HashiCorp Vault, or Harness secrets manager to inject secrets securely into your cluster.

---

## Monitoring and Networking

- Monitoring stack includes Grafana, Kubernetes Dashboard, and Uptime Kuma  
- Networking includes Traefik ingress controller, MetalLB for load balancing, cert-manager for TLS certificates, and external-dns for DNS management  

---

## Contributing

Contributions and improvements are welcome! Please open issues or pull requests with clear descriptions.

---

## License

This project is licensed under the [LICENSE](LICENSE) file.

---

## Contact

For questions or support, open an issue or reach out to the maintainer.
