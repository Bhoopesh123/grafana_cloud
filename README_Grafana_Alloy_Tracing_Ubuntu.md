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

# 3. Modify  configuration of Grafana Alloy

Copy the contents of the "config_tracing.alloy" configuration from this repo to your environment(/etc/alloy) and restart alloy agent

    sudo systemctl restart alloy
    sudo journalctl -u alloy -f

# 4. Install Application (Petclinic) on Ubuntu Machine: 
Reference Documentation: https://github.com/spring-projects/spring-petclinic  

Please follow the below commands to install Springboot Application

    git clone https://github.com/spring-projects/spring-petclinic.git
    cd spring-petclinic

Create the maven package

    ./mvnw package -Dmaven.test.skip=true 

Download opentelemetry java agent for Auto Instrumentation  
Reference Documentation: 

https://github.com/open-telemetry/opentelemetry-java-instrumentation 

    wget https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/latest/download/opentelemetry-javaagent.jar


Set OTEL Agent Environment Variables  

    export OTEL_EXPORTER_OTLP_TRACES_ENDPOINT=http://localhost:4318/v1/traces

Start the Petclinic Application

    java -javaagent:./opentelemetry-javaagent.jar -Dotel.service.name=petclinic -jar target/*.jar

Check the application on port like as below:  

    http://localhost:8080

# 6. Validate the Traces on Grafana  Cloud

Go to explore and verify the traces from Tempo Datasource