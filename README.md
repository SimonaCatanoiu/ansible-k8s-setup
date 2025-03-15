# 🚀 Kubernetes Cluster Setup with Ansible

## 📌 Overview
Install a Baremetal High-Availability Kubernetes Cluster using Ansible and kubeadm. This playbook automates the setup process and ensures a robust, production-ready environment with:
- **Containerd** as the container runtime.
- **Cilium** as the CNI (Container Network Interface).
- **HAProxy & Keepalived** for control plane load balancing and failover.

📌 **Note:** A **Jumphost** is used in the playbook to connect to all nodes.

---

## 🏗 Architecture
The cluster consists of the following components:

### 🔹 HAProxy Nodes (`haproxy` group)
- Load balancers using **HAProxy** for API server traffic.
- **Keepalived** ensures high availability via a virtual IP (VIP).

### 🔹 Kubernetes Master Nodes (`masters` group)
- Control plane nodes running `kube-apiserver`, `kube-controller-manager`, `kube-scheduler`, and `etcd`.

### 🔹 Kubernetes Worker Nodes (`workers` group)
- Nodes responsible for running application workloads.

### 🔹 Group Relationships
```ini
[k8s:children]
masters
workers

[nodes:children]
k8s
haproxy
```
---

## 📋 Inventory File Format
```ini
[haproxy]
web01 ansible_host=<ip> ansible_port=22 ansible_user=ubuntu ansible_ssh_private_key_file=<path_to_private_key>
web02 ansible_host=<ip> ansible_port=22 ansible_user=ubuntu ansible_ssh_private_key_file=<path_to_private_key>

[masters]
master1 ansible_host=<ip> ansible_port=22 ansible_user=ubuntu ansible_ssh_private_key_file=<path_to_private_key>
master2 ansible_host=<ip> ansible_port=22 ansible_user=ubuntu ansible_ssh_private_key_file=<path_to_private_key>
master3 ansible_host=<ip> ansible_port=22 ansible_user=ubuntu ansible_ssh_private_key_file=<path_to_private_key>

[workers]
worker1 ansible_host=<ip> ansible_port=22 ansible_user=ubuntu ansible_ssh_private_key_file=<path_to_private_key>
worker2 ansible_host=<ip> ansible_port=22 ansible_user=ubuntu ansible_ssh_private_key_file=<path_to_private_key>

[kvip]
web01

[all:vars]
ansible_ssh_common_args='-o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ProxyCommand="ssh -i <path_to_jumphost_key> -q -W %h:%p ubuntu@<jumphost_floating_ip>"'
```
---

## 🔧 Playbook Execution Flow

### 🔘 **General Setup**:
- Applies basic system configurations to **all nodes**.

### 🔘 **Kubernetes Prerequisites**
- Installs dependencies required for Kubernetes components.

### 🔘 **Containerd Installation**
- Installs and configures **containerd** as the container runtime.

### 🔘 **Kubernetes Installation**
- Installs `kubeadm`, `kubelet`, and `kubectl`.

### 🔘 **HAProxy & Keepalived Setup**
- **HAProxy** for API server load balancing.
- **Keepalived** for a virtual IP, ensuring control plane high availability.

### 🔘 **Master Node Initialization**
- The **first master** node initializes the cluster.

### 🔘 **Worker Nodes Join**
- Worker nodes join the cluster using `kubeadm`.

### 🔘 **Additional Master Nodes Join**
- The remaining master nodes join to create a **HA control plane**.

### 🔘 **Final Configuration**
- Configures **terminal access** and other essential settings.

---

## ⚠️ Missing Elements & Improvements
🔸 **Templating for HAProxy & Keepalived** 🔸
- HAProxy and Keepalived configuration files should be dynamically generated using **Jinja templates**.

🔸 **Reorganization of the Playbook** 🔸
- The playbook structure should be improved for better readability and maintainability.
---

## 🚀 Usage Instructions

### 1️⃣ Clone the Repository
```bash
git clone <repo_url>
cd <repo_directory>
```

### 2️⃣ Update the Inventory File
Modify `inventory.ini` to reflect your infrastructure details.

### 3️⃣ Run the Playbook
```bash
ansible-playbook -i inventory.ini site.yaml
```

---

## 🔮 Future Enhancements
✅ Implement **HAProxy & Keepalived Jinja templates**.


## 💬 Contributions & Feedback
Suggestions are wellcomed! Feel free to open issues or submit pull requests to improve this playbook. 🚀






