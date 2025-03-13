# Task 3: Deploy Application in Kubernetes

## Overview
In this task, I deployed the application in a Kubernetes cluster using the Docker image created in **Task 2**. The deployment includes the following:
- Running the application on two nodes.
- Setting up **CloudNativePG** with Persistent Volume (PV) and Persistent Volume Claim (PVC).
- Using **HashiCorp Vault** for Kubernetes secrets.
- Applying **Taint and Toleration** for node scheduling.
- Using **Istio** as the ingress and service mesh.
- Defining **resource requests and limits** for the application.
- Implementing **fixed scaling** for the application.
- Allocating **2 GB of storage** for the application.

---

## Steps to Deploy the Application in Kubernetes

### 1. **Create Kubernetes Manifest Files**
- Created the following Kubernetes manifest files:
    - `todo_deployment.yaml`: To deploy the application.
    - `service.yaml`: To expose the application.
    - `ingress.yaml`: To configure Istio ingress.
    - `cloudnativepg_pv_pvc.yaml`: To set up CloudNativePG with PV and PVC.
    - `vault-secrets.yaml`: To integrate HashiCorp Vault for secrets management.

### 2. **Deploy the Application**
- Applied the deployment manifest to run the application on two nodes:
    ```bash
    k apply -f todo_todo_deployment.yaml
    ```
- Verified the deployment status:
    ```bash
    k get deployments
    ```

### 3. **Set Up CloudNativePG with PV and PVC**
- Created a Persistent Volume (PV) and Persistent Volume Claim (PVC) for CloudNativePG:
    ```bash
    k apply -f cloudnativepg_pv_pvc.yaml
    ```
- Verified the PV and PVC status:
    ```bash
    k get pv
    k get pvc
    ```

### 4. **Integrate HashiCorp Vault for Secrets**
- Set up HashiCorp Vault in the Kubernetes cluster:
    ```bash
    k apply -f vault-secrets.yaml
    ```
- Verified the Vault secrets:
    ```bash
    k get secrets
    ```

### 5. **Apply Taint and Toleration**
- Applied taint to a specific node to control scheduling:
    ```bash
    k taint nodes <node-name> key=value:NoSchedule
    ```
- Added toleration in the deployment manifest to allow the application to run on the tainted node.

### 6. **Configure Istio as Ingress and Service Mesh**
- Installed Istio in the Kubernetes cluster:
    ```bash
    istioctl install --set profile=default -y
    ```
- Applied the Istio ingress configuration:
    ```bash
    k apply -f ingress.yaml
    ```
- Verified the Istio ingress status:
    ```bash
    k get svc -n istio-system
    ```

### 7. **Define Resource Requests and Limits**
- Added resource requests and limits in the `todo_deployment.yaml` file:
    ```yaml
    resources:
        requests:
            memory: "512Mi"
            cpu: "500m"
        limits:
            memory: "1Gi"
            cpu: "1"
    ```

### 8. **Implement Fixed Scaling**
- Configured fixed scaling in the `todo_deployment.yaml` file to run two replicas of the application:
    ```yaml
    replicas: 2
    ```

### 9. **Allocate 2 GB of Storage**
- Allocated 2 GB of storage for the application by configuring the PVC in `cloudnativepg_pv_pvc.yaml`:
    ```yaml
    resources:
        requests:
            storage: 2Gi
    ```

### Verification
- Verified the application deployment:
    ```bash
    k get pods
    ```
- Verified the Istio ingress:
    ```bash
    k get ingress
    ```
- Verified the CloudNativePG setup:
    ```bash
    k get pv
    k get pvc
    ```

![Screenshot of the exposed to do app:](https://github.com/ZuhairHossain/devops-todo-app/blob/master/task3/images/todo_app_running.png)

### Challenges Faced
- **Resource Constraints**: Had to carefully allocate CPU and memory resources to avoid overloading the cluster.
- **Istio Configuration**: Required additional setup and troubleshooting to configure Istio as the ingress and service mesh.
- **HashiCorp Vault Integration**: Needed to ensure proper secret management and access control.
