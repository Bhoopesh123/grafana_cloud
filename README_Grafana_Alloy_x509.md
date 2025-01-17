# x509-exporter for monitoring expired x509 certificates  

A Prometheus exporter for certificates focusing on expiration monitoring, written in Go. Designed to monitor Kubernetes clusters from inside, it can also be used as a standalone exporter.

Get notified before they expire:

    PEM encoded files, by path or scanning directories
    Kubeconfigs with embedded certificates or file references
    TLS Secrets from a Kubernetes cluster

# 1. Install X509 Exporter  

    helm repo add enix https://charts.enix.io
    helm upgrade --install x509-certificate-exporter enix/x509-certificate-exporter

# 2. Install Alloy  

Make sure to update the x509 exporter service ip in alloy configuration in configmap_x509.alloy file before creating configmap

    kubectl create configmap --namespace metrics alloy-config "--from-file=configmap_x509.alloy=./configmap_x509.alloy"

    helm upgrade --install  --namespace metrics alloy grafana/alloy -f grafana_x509_alloy.yaml

# 3. Validate the metrics  

To Search all of the time series data points grouping by job  

    count({__name__=~".+"}) by (job)

# 4. Import the x509 Dashboard  

Import the dashboard by following below reference documentation
    https://grafana.com/grafana/dashboards/13922-certificates-expiration-x509-certificate-exporter/

    Dashboard id: 13922