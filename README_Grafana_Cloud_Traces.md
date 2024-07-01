# Grafana Cloud  
Create Grafana Cloud Account which is valid for 15 Days  

Reference Documentation:  

    https://grafana.com/products/cloud/

# 1. Install Tempo on Ubuntu Monitoring Server:   
Install it by following below commands:  

    curl -Lo tempo_2.2.0_linux_amd64.deb https://github.com/grafana/tempo/releases/download/v2.2.0/tempo_2.2.0_linux_amd64.deb
    echo e81cb4ae47e1d8069efaad400df15547e809b849cbb18932e23ac3082995535b \
    tempo_2.2.0_linux_amd64.deb | sha256sum -c
    dpkg -i tempo_2.2.0_linux_amd64.deb

# 2. Modify the Tempo configuration.

Copy the contents of the "tempo_config.yml" configuration from this repo to your environment at /etc/tempo/config.yml and change the api key and url details.

# 4. Restart the Tempo

    sudo systemctl stop tempo.service
    sudo systemctl start tempo.service
    sudo systemctl is-active tempo
    sudo systemctl status tempo.service

Validate the service logs by below command:

    journalctl -u tempo.service -f

# 5. Test the Tempo Traces by sample application  

Kindly replace the below one with your url  

    git clone https://github.com/grafana/tempo.git
    cd /etc/tempo/tempo/example/docker-compose/shared
Repalce the ip with the machine ip for routing metrics and traces from the application.

    cd /etc/tempo/tempo/example/docker-compose/local
    grafana-datasources.yaml and prometheus.yaml

Copy the contents of the "docker-compose.yaml" configuration from this repo to your environment at /etc/tempo and change the api key and url details.

    docker compose up -d
    sudo systemctl restart tempo
