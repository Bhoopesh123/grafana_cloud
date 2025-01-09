# Grafana Alloy  
Alloy is a flexible, high performance, vendor-neutral distribution of the OpenTelemetry (OTel) Collector. It’s fully compatible with the most popular open source observability standards such as OpenTelemetry (OTel) and Prometheus.

Reference Documentation:  

    https://grafana.com/docs/alloy/latest/introduction/

# 1. Install Minikube cluster on Ubuntu

https://github.com/Bhoopesh123/k3d-minikube/blob/main/README.md

# 2. Install Black-Box Exporter on Cluster  

    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    helm repo update
    helm upgrade --install prometheus-blackbox prometheus-community/prometheus-blackbox-exporter -f black-values.yaml

# 3. Install Alloy on Minikube Cluster 

Install it by following below commands:  

    helm repo add grafana https://grafana.github.io/helm-charts
    helm repo update
    kubectl create namespace metrics
    helm install --namespace metrics alloy grafana/alloy
    kubectl get pods --namespace metrics

# 4. Configure and Run Alloy on Minikube Cluster

Make sure to update the node_exporter service ip in alloy configuration in configmap_bbexporter file before creating configmap

    kubectl create configmap --namespace metrics alloy-config "--from-file=configmap_bbexporter.alloy=./configmap_bbexporter.alloy"

    helm upgrade --install  --namespace metrics alloy grafana/alloy -f grafana_bb_alloy.yaml

# 5. Validate the metrics on Grafana

To Search all of the time series data points grouping by job  in query  

    count({__name__=~".+"}) by (job)

# 5. Add the Dashboard to Grafana:  

    https://grafana.com/grafana/dashboards/7587-prometheus-blackbox-exporter/

    Add id: 7587 to Grafana page
