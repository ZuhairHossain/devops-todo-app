# Task 5: Monitor Kubernetes Cluster

## Overview
In this task, the goal was to create **metrics visualizations** of the Kubernetes cluster using the **ELK stack** (Elasticsearch, Logstash, Kibana) and set up **alerting** for the cluster metrics. However, due to resource constraints on my local PC, I was unable to complete this task. Despite this, I have prior experience with **Elastalert** for setting up alerts using **SMTP** and **SMSC** in my workplace, and I am confident in implementing similar solutions if given the opportunity.

---

## Steps I Would Have Taken to Complete Task 5

### 1. **Set Up Metricbeat for Kubernetes Metrics**
- Deploy **Metricbeat** in the Kubernetes cluster to collect metrics (e.g., CPU, memory, disk usage) from the nodes and pods.
- Apply the Metricbeat configuration using a Kubernetes manifest:
    ```bash
    kubectl apply -f metricbeat.yaml
    ```

### 2. **Ship Metrics to Elasticsearch**
- Configure Metricbeat to ship the collected metrics to Elasticsearch.

### 3. **Create Visualizations in Kibana**
- Use Kibana to create visualizations for the Kubernetes cluster metrics (e.g., CPU usage, memory usage, pod status).
- Create dashboards to monitor the health and performance of the cluster.

### 4. **Set Up Alerting with Elastalert**
- Deploy Elastalert in the Kubernetes cluster to set up alerts based on the metrics collected by Metricbeat.
- Configure Elastalert to send alerts via SMTP (email) and SMSC (SMS).

## Why I Couldn't Complete This Task
- **Resource Constraints**: My local PC does not have sufficient resources (CPU, memory) to run the ELK stack alongside the Kubernetes cluster and Metricbeat.
- **System Instability**: Attempting to run Elasticsearch and Kibana caused my system to hang and crash due to insufficient memory.

## My Experience with Alerting (Elastalert)
In my workplace, I have successfully implemented Elastalert for monitoring and alerting.
- Configured alerts to notify the team via SMTP (email) and SMSC (SMS) for critical issues such as high CPU usage, memory spikes, and service downtime.

### Examples of alerts I have set up:
- API usage more than a threshold value
- Failed transaction rate of any service
- High CPU usage (> 85% for 5 minutes).
- Memory usage exceeding 85%.
- Pod crashes or restarts.
