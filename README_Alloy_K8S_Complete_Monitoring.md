# Grafana Alloy  
Alloy is a flexible, high performance, vendor-neutral distribution of the OpenTelemetry (OTel) Collector. It’s fully compatible with the most popular open source observability standards such as OpenTelemetry (OTel) and Prometheus.

Reference Documentation:  

    https://grafana.com/docs/alloy/latest/introduction/

https://grafana.com/docs/grafana-cloud/monitor-infrastructure/kubernetes-monitoring/configuration/helm-chart-config/

# 1. Install Minikube cluster on Ubuntu

https://github.com/Bhoopesh123/k3d-minikube/blob/main/README.md

# 2. Go to Configuration of Kubernetes Page of Grafana Cloud

Install the below Grafana Helm Chart in Cluster  

        helm repo add grafana https://grafana.github.io/helm-charts &&
        helm repo update &&
        helm upgrade --install --version ^1 --atomic --timeout 300s grafana-k8s-monitoring grafana/k8s-monitoring \
            --namespace "metrics" --create-namespace --values - <<EOF
        cluster:
        name: my-cluster
        externalServices:
        prometheus:
            host: https://prometheus-prod-13-prod-us-east-0.grafana.net
            basicAuth:
            username: "1646953"
            password: glc_eyJvIjoiMTE1NjExOCIsIm4iOiJzdGFjay05NjU2MjAtaW50ZWdyYXRpb24tY29tcGxldGVfbW9uaXRvcmluZzItY29tcGxldGVfbW9uaXRvcmluZzIiLCJrIjoiMzM4UTVzZVlJM2NvZzkxY3BBZjk4MXJ2IiwibSI6eyJyIjoicHJvZC11cy1lYXN0LTAifX0=
        loki:
            host: https://logs-prod-006.grafana.net
            basicAuth:
            username: "923400"
            password: glc_eyJvIjoiMTE1NjExOCIsIm4iOiJzdGFjay05NjU2MjAtaW50ZWdyYXRpb24tY29tcGxldGVfbW9uaXRvcmluZzItY29tcGxldGVfbW9uaXRvcmluZzIiLCJrIjoiMzM4UTVzZVlJM2NvZzkxY3BBZjk4MXJ2IiwibSI6eyJyIjoicHJvZC11cy1lYXN0LTAifX0=
        tempo:
            host: https://tempo-prod-04-prod-us-east-0.grafana.net:443
            basicAuth:
            username: "917716"
            password: glc_eyJvIjoiMTE1NjExOCIsIm4iOiJzdGFjay05NjU2MjAtaW50ZWdyYXRpb24tY29tcGxldGVfbW9uaXRvcmluZzItY29tcGxldGVfbW9uaXRvcmluZzIiLCJrIjoiMzM4UTVzZVlJM2NvZzkxY3BBZjk4MXJ2IiwibSI6eyJyIjoicHJvZC11cy1lYXN0LTAifX0=
        metrics:
        enabled: true
        alloy:
            metricsTuning:
            useIntegrationAllowList: true
        cost:
            enabled: true
        kepler:
            enabled: true
        node-exporter:
            enabled: true
        beyla:
            enabled: true
        logs:
        enabled: true
        pod_logs:
            enabled: true
        cluster_events:
            enabled: true
        traces:
        enabled: true
        receivers:
        grpc:
            enabled: true
        http:
            enabled: true
        zipkin:
            enabled: true
        grafanaCloudMetrics:
            enabled: true
        opencost:
        enabled: true
        opencost:
            exporter:
            defaultClusterId: my-cluster
            prometheus:
            external:
                url: https://prometheus-prod-13-prod-us-east-0.grafana.net/api/prom
        kube-state-metrics:
        enabled: true
        prometheus-node-exporter:
        enabled: true
        prometheus-operator-crds:
        enabled: true
        kepler:
        enabled: true
        alloy: {}
        alloy-events: {}
        alloy-logs: {}
        beyla:
        enabled: true
        EOF

# 3. Install Application on Minikube cluster:    
Please refer the below page to install Prometheus on Cluster: 

    cd app_monitoring 
    
    kubectl apply -f .

# 6. Validate the metrics on Grafana
To Search all of the time series data points grouping by job  in query  

    count({__name__=~".+"}) by (job)

# 7. Add Beyla Dashboard

    Add the below dashboard id 19923

