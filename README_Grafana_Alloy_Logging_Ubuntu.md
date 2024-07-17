# Grafana Alloy  
Alloy is a flexible, high performance, vendor-neutral distribution of the OpenTelemetry (OTel) Collector. It’s fully compatible with the most popular open source observability standards such as OpenTelemetry (OTel) and Prometheus.

Reference Documentation:  

    https://grafana.com/docs/alloy/latest/introduction/

# 1. Install Alloy on Ubuntu Monitoring Server
Install it by following below commands:  

    sudo apt install gpg
    sudo mkdir -p /etc/apt/keyrings/
    wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
    echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee /etc/apt/sources.list.d/grafana.list
    sudo apt-get update
    sudo apt-get install alloy


# 2. Configure and Run Alloy on Ubuntu Monitoring Server

    sudo systemctl reload alloy
    sudo systemctl start alloy
    sudo systemctl status alloy
    sudo systemctl enable alloy.service
    sudo systemctl restart alloy
    sudo systemctl stop alloy
    sudo journalctl -u alloy -f

# 3. Install and Configure Loki on Ubuntu Machine

    https://github.com/Bhoopesh123/Grafana_onprem/blob/main/README.md

# 4. Modify Logging configuration of Grafana Alloy

Copy the contents of the "config_logging.alloy" configuration from this repo to your environment(/etc/alloy) and restart alloy agent

    sudo systemctl restart alloy
    sudo journalctl -u alloy -f

# 6. Validate the Logs on Grafana  

Go to explore and verify the logs from Loki Datasource