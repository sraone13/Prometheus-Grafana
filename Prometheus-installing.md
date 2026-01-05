# Prometheus Installation on Kubernetes (Using Helm)

### Step 1: Setup Kubernetes Cluster
Ensure Kubernetes cluster is up and running.

### Step 2: Add Prometheus Helm Repository
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```
### Step 3: Update Helm Repositories
```
helm repo update
```
### Step 4: Install Prometheus
```
helm install prometheus prometheus-community/prometheus
```
### Step 5: Verify Installation
```
kubectl get pods
kubectl get svc
```
**If all the prometheus-server and prometheus-alertmanager pods is not running**

Values.yaml
```
server:
  persistentVolume:
    enabled: false
  resources:
    requests:
      cpu: 300m
      memory: 512Mi
    limits:
      cpu: 600m
      memory: 1Gi

alertmanager:
  persistence:
    enabled: false
  resources:
    requests:
      cpu: 100m
      memory: 128Mi
    limits:
      cpu: 300m
      memory: 256Mi
  ```

Install the Prometheus with this value.yaml file
```
helm install prometheus prometheus-community/prometheus -f values.yaml
```

### Step 6: Expose Prometheus Server (NodePort)
```
kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-ext
```
### Step 7: Access Prometheus UI

Get Node IP:
```
kubectl get nodes -o wide
```


Get NodePort:
```
kubectl get svc
```

Access in browser:

http://<NodeIP>:<NodePort>

### Step 8: Prometheus UI

Use PromQL queries to view metrics.

Metrics are time-series based (not logs).


### Simple PromQL queries

🔹 All pods
```
kube_pod_info
```
🔹 Pods by namespace
```
kube_pod_info{namespace="default"}
```
🔹 Only non-running pods
```
kube_pod_status_phase{phase!="Running"} == 1
```

**7️⃣ List Pods with CPU & Memory (PromQL)**
```
sum by (pod, namespace) (
  container_cpu_usage_seconds_total
)
```
```
sum by (pod, namespace) (
  container_memory_working_set_bytes
)
```
