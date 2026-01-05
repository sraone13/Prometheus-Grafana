# Grafana Installation on Kubernetes (Using Helm)

### Step 1: Add Grafana Helm Repository
```
helm repo add grafana https://grafana.github.io/helm-charts
```
### Step 2: Update Helm Repositories
```
helm repo update
```
### Step 3: Install Grafana
```
helm install grafana grafana/grafana
```
### Step 4: Get Grafana Admin Password
```
kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode; echo
```
Username: admin
Password: (output of above command)

### Step 5: Expose Grafana (NodePort)
```
kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-ext
```
### Step 6: Access Grafana UI
```
kubectl get svc
```

Open in browser:

http://<NodeIP>:<NodePort>

### 4. Add Prometheus as Data Source in Grafana

Login to Grafana UI

Go to Configuration → Data Sources

Click Add data source

Select Prometheus

Enter Prometheus URL:

http://prometheus-server-ext:9090


(or NodeIP + NodePort)
6. Click Save & Test

### 5. Import Kubernetes Dashboard in Grafana
Steps

Go to Dashboards → Import

Enter Dashboard ID: 3662

Click Load

Select Prometheus data source

Click Import

Result

Kubernetes cluster dashboards will be visible

You can monitor:

Nodes

Pods

CPU & Memory usage

Cluster health


### Grafana dashboard names with their import codes for Kubernetes monitoring:

Kubernetes Cluster Monitoring        – 1860
Kubernetes Pods                      – 315
Kubernetes Nodes                     – 11074
Kubernetes / Compute Resources / Pod – 15757
Kubernetes / Compute Resources / Node– 8588
