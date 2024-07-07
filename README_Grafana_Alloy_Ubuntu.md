# Grafana Alloy  
Alloy is a flexible, high performance, vendor-neutral distribution of the OpenTelemetry (OTel) Collector. It’s fully compatible with the most popular open source observability standards such as OpenTelemetry (OTel) and Prometheus.

Reference Documentation:  

    https://grafana.com/docs/alloy/latest/introduction/

# 1. Install Alloy on Ubuntu Monitoring Server:   
Install it by following below commands:  

    sudo apt install gpg
    sudo mkdir -p /etc/apt/keyrings/
    wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
    echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee /etc/apt/sources.list.d/grafana.list
    sudo apt-get update
    sudo apt-get install alloy


# 2. Configure and Run Alloy on Ubuntu Monitoring Server:.

    sudo systemctl reload alloy
    sudo systemctl start alloy
    sudo systemctl status alloy
    sudo systemctl enable alloy.service
    sudo systemctl restart alloy
    sudo systemctl stop alloy
    sudo journalctl -u alloy -f


# 4. Migrate from Prometheus to Grafana Alloy

To fully migrate your configuration from Prometheus to Alloy, you must convert your Prometheus configuration into an Alloy configuration. This conversion will enable you to take full advantage of the many additional features available in Alloy.

    alloy convert --source-format=prometheus --output=<OUTPUT_CONFIG_PATH> <INPUT_CONFIG_PATH>

<INPUT_CONFIG_PATH>: The full path to the Prometheus configuration.
<OUTPUT_CONFIG_PATH>: The full path to output the Alloy configuration.

# 5. Copy paste the converted file to alloy directory

Copy the contents of the "config.alloy" configuration from this repo to your environment(/etc/alloy) and restart alloy agent

    sudo systemctl restart alloy
    sudo journalctl -u alloy -f

# 6. Validate the metrics on Prometheus and Grafana

To Search all of the time series data points grouping by job  in query  

  count({__name__=~".+"}) by (job)