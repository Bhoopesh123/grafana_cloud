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
    kubectl create namespace metrics
    helm install --namespace metrics alloy grafana/alloy
    kubectl get pods --namespace metrics

# 4. Configure and Run Alloy on Minikube Cluster

    Make sure to update the node_exporter service ip in alloy configuration in configmap.alloy file before creating configmap

    kubectl create configmap --namespace metrics alloy-config "--from-file=config_logging_Pods_k8s.alloy=./config_logging_Pods_k8s.alloy"

    helm  upgrade --install --namespace metrics alloy grafana/alloy -f grafana_alloy_k8s_pod_values.yaml

# 5. Validate the Logs on Grafana

Validate Logs on Loki via Explore Section