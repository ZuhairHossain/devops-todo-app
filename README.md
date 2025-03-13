# DevOps Exam Submission

## Task 1: Provision a Kubernetes Cluster

### Completed Subtasks:
1. **Set up a Kubernetes cluster with one master and two nodes:**
   - Used `kubeadm` to initialize the Kubernetes cluster.
   - Joined two worker nodes to the master node.
   - Verified the cluster status using `kubectl get nodes`.

2. **Created Persistent Volumes (PV) and Persistent Volume Claims (PVC):**
   - Created YAML files for PV and PVC.
   - Applied the configurations using `kubectl apply -f pv-pvc.yaml`.
   - Verified the PV and PVC status using `kubectl get pv` and `kubectl get pvc`.

### Incomplete Subtask:
3. **View metrics like CPU & memory with ELK:**
   - **Reason for Incompletion:** Due to resource constraints, I was unable to set up the ELK stack (Elasticsearch, Logstash, Kibana) for monitoring CPU and memory metrics.

---

## Task 2: Optimize and Deploy a Docker Image

### Completed Subtasks:
1. **Cloned the repository from GitHub:**
   - Cloned the repository using:
     ```bash
     git clone https://github.com/zahido/devops-exam.git
     ```

2. **Created an optimized Dockerfile for the application:**
   - Optimized the Dockerfile by reducing layers and using multi-stage builds.
   - Dockerfile:
     ```Dockerfile
        # Stage 1: Build the application
        FROM node:16-alpine as builder
        WORKDIR /app
        COPY package.json yarn.lock ./
        RUN yarn install
        COPY . .
        RUN yarn test

        # Stage 2: Create the production image
        FROM node:16-alpine
        WORKDIR /app
        COPY --from=builder /app/node_modules ./node_modules
        COPY --from=builder /app/src ./src
        COPY --from=builder /app/package.json ./
        ENV NODE_ENV=production
        EXPOSE 3000
        CMD ["node", "src/index.js"]
     ```

3. **Pushed the optimized Docker image to Docker Hub:**
   - Built the Docker image:
     ```bash
     docker build -t zuhairhossain/todo-app:v1 .
     ```
   - Pushed the image to Docker Hub:
     ```bash
     docker push zuhairhossain/todo-app:v1
     ```
   - **Docker Hub Image:** [zuhairhossain/todo-app](https://hub.docker.com/r/zuhairhossain/todo-app)

---

## Task 3: Deploy Application in Kubernetes

### Completed Subtasks:
1. **Created Kubernetes manifest files:**
   - Created `deployment.yaml`, `service.yaml`, and `ingress.yaml` files for the application.
   - Applied the configurations using `kubectl apply -f deployment.yaml`.

2. **Configured CloudNativePG with PV and PVC:**
   - Created YAML files for CloudNativePG, PV, and PVC.
   - Applied the configurations and verified the status.

3. **Integrated HashiCorp Vault for secrets management:**
   - Set up HashiCorp Vault in the Kubernetes cluster.
   - Created secrets and configured the application to use them.

4. **Used Istio as ingress and service mesh:**
   - Installed Istio in the Kubernetes cluster.
   - Configured Istio ingress and service mesh for the application.

5. **Defined resource requests and limits:**
   - Added resource requests and limits in the `deployment.yaml` file.
   - Example:
     ```yaml
     resources:
       requests:
         memory: "512Mi"
         cpu: "500m"
       limits:
         memory: "1Gi"
         cpu: "1"
     ```

6. **Implemented fixed scaling for the application:**
   - Configured the application to run on two nodes using `replicas: 2` in the deployment file.

7. **Allocated 2 GB of storage for the application:**
   - Configured the PVC to allocate 2 GB of storage.

---

## Task 4: Log Management

### Incomplete Task:
- **Reason for Incompletion:** Due to resource constraints, I was unable to set up the ELK stack for log management. This includes:
  - Shipping logs from the Demo App to ELK.
  - Creating a dashboard in Kibana.
  - Implementing a 2-day log retention policy.

---

## Task 5: Monitor Kubernetes Cluster

### Incomplete Task:
- **Reason for Incompletion:** Due to resource constraints, I was unable to set up the ELK stack for monitoring Kubernetes cluster metrics and creating alerts.

---

## Challenges Faced
- **Resource Constraints:** The primary challenge was the lack of sufficient resources (CPU, memory, and storage) to set up the ELK stack for monitoring and log management.
---

## Conclusion
While I was able to complete most of the tasks, I faced challenges with resource-intensive tasks like setting up the ELK stack in my local PC. I have documented all completed tasks and provided explanations for the incomplete ones. If given more resources and time, I would be able to complete the remaining tasks.

---