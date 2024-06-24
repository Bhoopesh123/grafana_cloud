# Grafana Cloud  
Create Grafana Cloud Account which is valid for 15 Days  

Reference Documentation:  

    https://grafana.com/products/cloud/

# 1. Install Prometheus on Ubuntu Monitoring Server:   
Install it by following Prometheus.yml file commands from below documentation:  

    https://github.com/Bhoopesh123/Grafana_onprem  

# 2. Install NodeExporter on Client Machine  
Install it by following NodeExporter.yml file commands from below documentation:   

    https://github.com/Bhoopesh123/Grafana_onprem 

# 3. Modify the prometheus configuration.

    cd /etc/prometheus

Copy the contents of the "prometheus.yml" configuration from this repo to your environment and change the api key and url details.

# 4. Restart the Prometheus agent

    sudo systemctl stop prometheus
    sudo systemctl start prometheus
    sudo systemctl status prometheus

Validate the service logs by below command:

    journalctl -u prometheus -f

# 5. Validate the metrics in Grafana cloud account  

Kindly replace the below one with your url  

    https://bhoopeshsharma.grafana.net/

To Search all of the time series data points grouping by job  in query  

  count({__name__=~".+"}) by (job)   

# 6. Import the Grafana Dashboards

Import 1860 dashboard id for Node Exporter metrics  

    https://grafana.com/grafana/dashboards/1860-node-exporter-full/