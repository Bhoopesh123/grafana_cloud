# Grafana Alloy  
Alloy is a flexible, high performance, vendor-neutral distribution of the OpenTelemetry (OTel) Collector. It’s fully compatible with the most popular open source observability standards such as OpenTelemetry (OTel) and Prometheus.

Reference Documentation:  

    https://grafana.com/docs/alloy/latest/introduction/

# 1. Install Minikube cluster on Ubuntu

https://github.com/Bhoopesh123/k3d-minikube/blob/main/README.md


# 2. Install Grafana-K8S Helm Chart on Minikube Cluster 

Install it by following below commands:  
Reference Documentation:  

https://github.com/grafana/k8s-monitoring-helm/blob/main/charts/k8s-monitoring/README.md

    helm repo add grafana https://grafana.github.io/helm-charts
    helm repo update
    helm upgrade --install --namespace metrics grafana-k8s-monitoring grafana/k8s-monitoring --values cluster_values.yaml

# 3. Validate the metrics and Logs on Grafana

To Search all of the time series data points grouping by job  in query  

    count({__name__=~".+"}) by (job)
