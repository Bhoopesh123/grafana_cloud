# Grafana Alloy  
Alloy is a flexible, high performance, vendor-neutral distribution of the OpenTelemetry (OTel) Collector. It’s fully compatible with the most popular open source observability standards such as OpenTelemetry (OTel) and Prometheus.

Reference Documentation:  

    https://grafana.com/docs/alloy/latest/introduction/

# 1. Install Minikube cluster on Ubuntu

https://github.com/Bhoopesh123/k3d-minikube/blob/main/README.md

# 2. Install Prometheus Node Exporter on Minikube Cluster

    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    helm repo update
    helm install node-exporter prometheus-community/prometheus-node-exporter

# 3. Install Alloy on Minikube Cluster 

Install it by following below commands:  

    helm repo add grafana https://grafana.github.io/helm-charts
    helm repo update
    helm install --namespace opentelemetry-demo alloy grafana/alloy
    kubectl get pods --namespace opentelemetry-demo

# 4. Install Application on Minikube cluster:    
Please refer the below page to install Prometheus on Cluster: 

    cd app_traces 
    
    kubectl apply -f .

# 5. Configure and Run Alloy on Minikube Cluster

Make sure to update the node_exporter service ip in alloy configuration in configmap.alloy file before creating configmap

    kubectl create configmap --namespace opentelemetry-demo alloy-config "--from-file=config_k8s_metrics_logs_tracing.alloy=./config_k8s_metrics_logs_tracing.alloy"

    helm  upgrade --install --namespace opentelemetry-demo alloy grafana/alloy -f alloy_k8s_app_monitoring.yaml

# 6. Validate the metrics on Grafana

To Search all of the time series data points grouping by job  in query  

    count({__name__=~".+"}) by (job)

# 7. Import the Node Exporter Dashboards

Import 1860 dashboard id for Node Exporter metrics  

    https://grafana.com/grafana/dashboards/1860-node-exporter-full/