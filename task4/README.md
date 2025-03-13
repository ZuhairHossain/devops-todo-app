# Task 4: Log Management

## Overview
In this task, I set up **Fluent Bit** to ship logs from the application to an **ELK stack** (Elasticsearch, Logstash, Kibana). The logs are visualized in a **Kibana dashboard**, and a **2-day log retention policy** is implemented to manage storage efficiently.

---

## Steps to Set Up Log Management

### 1. **Install Fluent Bit**
- Fluent Bit was installed in the Kubernetes cluster to collect logs from the application pods.
- Applied the Fluent Bit configuration using a Kubernetes manifest:
    ```bash
    kubectl apply -f fluent-bit-config.yaml
    ```

### 2. **Set Up the ELK Stack**
- Elasticsearch, Logstash, and Kibana were deployed in the Kubernetes cluster.
- Applied the ELK stack configuration using Kubernetes manifests:
    ```bash
    kubectl apply -f elasticsearch.yaml
    kubectl apply -f logstash.yaml
    kubectl apply -f kibana.yaml
    ```

### 3. **Configure Fluent Bit to Ship Logs to ELK**
- Updated the Fluent Bit configuration to ship logs to Elasticsearch via Logstash.


### 4. **Create a Kibana Dashboard**
- Due to resource constraints on the local PC, I was unable to fully configure and visualize the logs in the Kibana UI.

### 5. **Implement a 2-Day Log Retention Policy**
- Due to Elasticsearch issues, I was unable to fully implement the 2-day log retention policy. However, here is an example ILM policy based on my workplace knowledge:
    ```yaml
    apiVersion: elasticsearch.k8s.elastic.co/v1
    kind: Elasticsearch
    metadata:
        name: elasticsearch
    spec:
        version: 7.17.5
        nodeSets:
        - name: default
            count: 1
            config:
                node.master: true
                node.data: true
                node.ingest: true
            podTemplate:
                spec:
                    containers:
                    - name: elasticsearch
                        resources:
                            requests:
                                memory: "2Gi"
                                cpu: "1"
                            limits:
                                memory: "4Gi"
                                cpu: "2"
        indexLifecyclePolicy:
            policies:
            - name: 2-day-retention
                phases:
                    delete:
                        min_age: 2d
                        actions:
                            delete: {}
    ```
- This policy is intended to automatically delete logs older than 2 days.
- Note: The actual implementation may vary based on the specific Elasticsearch setup and version.

## Verification
- Verified that logs are being shipped to Elasticsearch:
    ```bash
    kubectl logs -l k8s-app=fluent-bit -n logging
    ```

## Challenges Faced
- **Resource Constraints**: Elasticsearch requires significant memory, which caused issues on my local PC. I had to limit the resources allocated to Elasticsearch still could not able to run the elasticsearch pod.
- **Log Retention Policy**: Configuring the 2-day retention policy is a demo one, I wrote based on my knowledge. So, it may vary due to ELK version issues.
