Kubernetes Cluster on Proxmox

**Project Goal**  
Build a lightweight, multi-node Kubernetes cluster using K3s on three virtual machines running inside Proxmox VE. This setup demonstrates core DevOps and platform engineering skills: infrastructure provisioning, networking, Kubernetes orchestration, and cluster management.

**Status**: Fully operational 3-node cluster (1 control plane + 2 workers)

**Date Completed**: January 2026

## Architecture Overview

- **Hypervisor**: Proxmox VE
- **Virtual Machines** (all connected to the same bridge `vmbr0`):
  | Role              | OS            | Hostname       | IP Address Example |
  |-------------------|---------------|----------------|--------------------|
  | Control Plane     | Ubuntu Server | k8s-master     | 192.168.1.101      |
  | Worker Node 1     | Kali Linux    | k8s-worker1    | 192.168.1.102      |
  | Worker Node 2     | Linux Mint    | k8s-worker2    | 192.168.1.103      |

- **Kubernetes Distribution**: K3s (lightweight, CNCF-certified)
- **Cluster Type**: Multi-node with high-availability potential
- **Networking**: Flannel (default CNI), Traefik ingress controller included

## Prerequisites Completed

- All VMs attached to the same Proxmox network bridge (`vmbr0`)
- Static or predictable IP addresses assigned
- VMs can ping each other by IP
- Swap disabled permanently on all nodes (`swapoff -a` + `/etc/fstab` edited)
- Hostnames set and basic system updates applied

## Installation Steps

### 1. On the Control Plane Node (Ubuntu)

```bash
# Install K3s as the first node (becomes control plane)
curl -sfL https://get.k3s.io | sh -

# Retrieve the node token for workers
sudo cat /var/lib/rancher/k3s/server/node-token

# Set up kubectl access for non-root user
mkdir ~/.kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
sudo chown $USER:$USER ~/.kube/config
