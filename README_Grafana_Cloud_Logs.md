# Grafana Cloud  
Create Grafana Cloud Account which is valid for 15 Days  

Reference Documentation:  

    https://grafana.com/products/cloud/

# 1. Install Promtail on Client Machine  
Install it by following Promtail.yml file commands from below documentation:   

    https://github.com/Bhoopesh123/Grafana_onprem 

# 2. Modify the Promtail configuration.

    cd /etc/

Copy the contents of the "promtail-local-config.yml" configuration from this repo to your environment and change the api key and url details.

# 3. Restart the Promtail agent

    sudo systemctl stop promtail.service
    sudo systemctl start promtail.service
    sudo systemctl status promtail.service

Validate the service logs by below command:

    journalctl -u promtail.service -f

# 4. Validate the logs in Grafana cloud account  

Kindly replace the below one with your url  

    https://bhoopeshsharma.grafana.net/

