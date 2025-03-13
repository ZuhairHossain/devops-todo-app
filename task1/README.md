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

### 8. **Verify Cluster Status**
- After setting up the cluster, verified the status of the nodes using:

    ```bash
    kubectl get nodes
    ```

![Screenshot of the created Kubernetes cluster:](task1\images\cluster_status.png)


# Persistent Volume (PV) and Persistent Volume Claim (PVC) Creation

## Overview
In this section, I created a **Persistent Volume (PV)** and **Persistent Volume Claim (PVC)** in the Kubernetes cluster. The PV provides storage for the cluster, and the PVC requests storage from the PV.

---

## Steps to Create PV and PVC

### 1. **Persistent Volume (PV)**
- Created a YAML file named `create_pv.yaml` to define a Persistent Volume (PV) with the following configuration:
  ```yaml
  apiVersion: v1
  kind: PersistentVolume
  metadata:
    name: local-pv
  spec:
    capacity:
      storage: 10Gi
    accessModes:
      - ReadWriteOnce
    persistentVolumeReclaimPolicy: Retain
    storageClassName: local-storage
    hostPath:
      path: "/mnt/data"  # This directory must exist on the node
      type: DirectoryOrCreate
    ```
 - Applied the PV configuration using:
    ```bash
    kubectl apply -f create_pv.yaml
    ```

### 2. **Persistent Volume Claim (PVC)**
- Created a YAML file named create_pvc.yaml to define a Persistent Volume Claim (PVC) with the following configuration:
    ```yaml
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
        name: local-pvc
    spec:
        accessModes:
            - ReadWriteOnce
    resources:
        requests:
            storage: 5Gi
    storageClassName: local-storage
  ```
- Applied the PVC configuration using:
    ```bash
    kubectl apply -f create_pvc.yaml
    ```

### 3. **Verify PV and PVC Status**
- Verified the status of the Persistent Volume (PV) using:
    ```bash
    k get pv
    ```
- Verified the status of the Persistent Volume Claim (PVC) using:
    ```bash
    k get pvc
    ```    
![Screenshot of the created Kubernetes PV & PVC:](task1\images\pv_and_pvc_status.png)

---

# ELK Stack Setup (Kibana and Logstash)

## Overview
#### As part of the task, I attempted to set up the ELK Stack (Elasticsearch, Logstash, Kibana) for log management and monitoring. However, due to memory constraints on my local PC (Windows 10), I was only able to successfully install Kibana and Logstash. Elasticsearch caused my system to hang and crash, so I could not complete its installation.

### Steps to Install Kibana and Logstash

### 1. **Kibana Installation**
- Created a YAML file named kibana.yaml to deploy Kibana in the Kubernetes cluster
- Applied the Kibana configuration using:
    ```bash
    k apply -f kibana.yaml
    ```
### 2. **Logstash Installation**
- Created a YAML file named logstash.yaml to deploy Logstash in the Kubernetes cluster:
- Applied the Logstash configuration using:
    ```bash
    k apply -f logstash.yaml
    ```

---
# Challenges Faced with Elasticsearch

### Memory Constraints: 
Elasticsearch requires significant memory to run. When I attempted to install Elasticsearch, my local PC (Windows 10) started hanging and eventually crashed due to insufficient memory.

### Resource Limitations
My local PC has 16 GB RAM and 4 cores CPU, which is not sufficient to run Elasticsearch alongside Kibana and Logstash in a Kubernetes cluster.

### Workaround Attempts 
I tried reducing the resource requests and limits for Elasticsearch, but it still caused system instability.

# Repository Structure for Task 1

task1/
├── manifests/
│   ├── kibana.yaml
│   ├── logstash.yaml
│   └── ...
├── create_pv.yaml
├── create_pvc.yaml
├── images/
│   ├── cluster_status.png
│   ├── pv_status.png
│   ├── pvc_status.png
│   ├── kibana_status.png
│   ├── logstash_status.png
│   └── ...
└── README.md