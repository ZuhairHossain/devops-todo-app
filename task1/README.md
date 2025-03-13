# Task 1: Provision a Kubernetes Cluster

## Overview
In this task, I set up a Kubernetes cluster with **one master node** and **two worker nodes** using VMware Pro on my local PC. The VMs were created with the following specifications:
- **Operating System:** Ubuntu 22.04
- **Storage:** 20 GB
- **CPU:** 2 cores
- **RAM:** 4 GB

My local PC has:
- **RAM:** 16 GB
- **CPU:** 4 cores

The cluster was created using `kubeadm`, and I followed the steps from this GitHub repository: [kubernetes-v1.30.2-cluster-using-kubeadm](https://github.com/Aareez01/kubernetes-v1.30.2-cluster-using-kubeadm).

---

## Steps to Set Up the Kubernetes Cluster

### 1. **Set Up VMware Pro and Install Ubuntu 22.04**
- Created three VMs in VMware Pro:
  - **master-1** (Master Node)
  - **worker-1** (Worker Node)
  - **worker-2** (Worker Node)
- Allocated **20 GB storage**, **2 cores CPU**, and **4 GB RAM** to each VM.

### 2. **Set Up SSH Access**
- Enabled SSH on each VM to access them from my local PC's terminal.
- Installed and configured SSH on each VM:
  ```bash
  sudo apt update
  sudo apt install openssh-server
  sudo systemctl enable ssh
  sudo systemctl start ssh
  ```

### 3. **Access VM from Local PC** 
-  As part of day to day tasks, I use termius, so I used that
    ```bash
    ssh test@192.168.0.116 [master-1]
    ssh test@192.168.0.117 [worker-1]
    ssh test@192.168.0.118 [worker-2]
    ```

### 4. **Change Hostname** 
-  Changed the hostname of each VM using the hostnamectl command:
    - For master-1:
    ```bash
    sudo hostnamectl set-hostname master-1
    ```
    - For worker-1:
    ```bash
    sudo hostnamectl set-hostname worker-1
    ```    
    - For worker-2:
    ```bash
    sudo hostnamectl set-hostname worker-2
    ```    

### 5. **Set Up Static IP for Each Node** 
-  Edited the 00-installer-config.yaml file on each VM to configure a static IP address.
    - Example configuration for master-1:
    ```bash
    sudo nano /etc/netplan/00-installer-config.yaml
    ```

    ```yaml
    network:
    version: 2
    renderer: networkd
    ethernets:
        ens33:
        dhcp4: no           # Disable DHCP
        addresses:
            - 192.168.0.116/24  # Static IP address (ensure it's within the same network range)
        gateway4: 192.168.0.1  # Default gateway (usually the router IP)
        nameservers:
            addresses:
            - 192.168.0.1      # DNS (usually the router IP, or you can use a public DNS like 8.8.8.8)
            - 8.8.8.8          # Google's public DNS
    ```
- Applied the network configuration:
    ```bash
    sudo netplan apply
    ```
- Verified the static IP configuration using:
    ```bash
    ip a
    ```

### 6. **Verify Internet Connectivity**
- Checked internet connectivity on each VM using:
    ```bash
    ping google.com
    ```
### 7. **Install Kubernetes Using ***kubeadm*****
- Followed the steps from this GitHub repository: [kubernetes-v1.30.2-cluster-using-kubeadm](https://github.com/Aareez01/kubernetes-v1.30.2-cluster-using-kubeadm).

- Installed Docker, kubeadm, kubelet, and kubectl on all nodes.

- Initialized the Kubernetes cluster on the master-1 node:

- Joined the worker nodes (worker-1 and worker-2) to the cluster using the kubeadm join command provided after initialization.

![See the attached screenshot](task1\images\Screenshot_1.png)

