# Grafana Alloy  
Alloy is a flexible, high performance, vendor-neutral distribution of the OpenTelemetry (OTel) Collector. It’s fully compatible with the most popular open source observability standards such as OpenTelemetry (OTel) and Prometheus.

Reference Documentation:  

    https://grafana.com/docs/alloy/latest/introduction/

# 1. Install Minikube cluster on Ubuntu

https://github.com/Bhoopesh123/k3d-minikube/blob/main/README.md

# 2. Install Alloy on Minikube Cluster 

Install it by following below commands:  

    helm repo add grafana https://grafana.github.io/helm-charts
    helm repo update
    kubectl create namespace metrics
    helm install --namespace metrics alloy grafana/alloy
    kubectl get pods --namespace metrics

# 3. Configure and Run Alloy on Minikube Cluster

    kubectl create configmap --namespace metrics alloy-config "--from-file=config_logging_Pods_k8s.alloy=./config_logging_Pods_k8s.alloy"

    helm  upgrade --install --namespace metrics alloy grafana/alloy -f grafana_alloy_k8s_pod_values.yaml

# 4. Validate the Logs on Grafana

Validate Logs on Loki via Explore Section