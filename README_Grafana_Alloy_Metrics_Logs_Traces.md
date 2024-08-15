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

Copy the contents of the "config_metrics_logs_traces.alloy" configuration from this repo to your environment(/etc/alloy) and restart alloy agent

    sudo systemctl restart alloy
    sudo journalctl -u alloy -f

# 4. Install Application (Petclinic) on Ubuntu Machine: 
Reference Documentation: https://github.com/spring-projects/spring-petclinic  

Please follow the below commands to install Springboot Application

    git clone https://github.com/spring-projects/spring-petclinic.git
    cd spring-petclinic

Add below dependencies in pom.xml for enabling metrics endpoint 

sudo vi pom.xml

    <!-- Spring and Spring Boot dependencies -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>

    <dependency>
      <groupId>io.micrometer</groupId>
      <artifactId>micrometer-registry-prometheus</artifactId>
    </dependency>


Add the following configuration to your application.properties file:

    cd spring-petclinic/src/main/resources
    sudo vi application.properties

    management.endpoints.web.exposure.include=*
    management.metrics.export.prometheus.enabled=true

Create the maven package

    ./mvnw package -Dmaven.test.skip=true 

Download opentelemetry java agent for Auto Instrumentation  
Reference Documentation: 
https://github.com/open-telemetry/opentelemetry-java-instrumentation 

    wget https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/latest/download/opentelemetry-javaagent.jar


Set OTEL Agent Environment Variables  

    export OTEL_METRIC_EXPORT_INTERVAL=5000 \
    && export OTEL_EXPORTER_OTLP_METRICS_TEMPORALITY_PREFERENCE=DELTA \
    && export OTEL_EXPORTER_OTLP_METRICS_DEFAULT_HISTOGRAM_AGGREGATION=BASE2_EXPONENTIAL_BUCKET_HISTOGRAM \
    && export OTEL_LOGS_EXPORTER=none \
    && export OTEL_EXPORTER_OTLP_ENDPOINT=http://localhost:4318 \
    && export OTEL_EXPORTER_OTLP_COMPRESSION=gzip \
    && export OTEL_EXPERIMENTAL_EXPORTER_OTLP_RETRY_ENABLED=true \
    && export OTEL_SERVICE_NAME=agent-nr-config \
    && export OTEL_RESOURCE_ATTRIBUTES=service.instance.id=1234 \
    && export OTEL_EXPERIMENTAL_RESOURCE_DISABLED_KEYS=process.command_line,process.command_args \
    && export OTEL_ATTRIBUTE_VALUE_LENGTH_LIMIT=4095 \
    && export OTEL_EXPORTER_OTLP_TRACES_ENDPOINT=http://localhost:4318/v1/traces \
    && export OTEL_TRACES_EXPORTER=otlp

Start the Petclinic Application

    java -jar target/*.jar 
    java -javaagent:./opentelemetry-javaagent.jar -Dotel.service.name=petclinic -jar target/*.jar

Check the application on port like as below:  

http://localhost:8080

# 5. Creating some files for Logging Prespective

    echo 'level=info msg="INFO: This is an info level log!"' > /tmp/alloy-logs/log.log
    echo 'level=warn msg="WARN: This is a warn level log!"' >> /tmp/alloy-logs/log.log
    echo 'level=debug msg="DEBUG: This is a debug level log!"' >> /tmp/alloy-logs/log.log

# 6. Validate the Metrics Logs and Traces on Grafana  Cloud

To Search all of the time series data points grouping by job  in query  

    count({__name__=~".+"}) by (job)

# 7. Server Requests count for App endpoints
  
    http_server_requests_seconds_count{job="petclinic_metrics",status!="200"}