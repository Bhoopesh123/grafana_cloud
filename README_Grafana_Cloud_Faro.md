# Grafana Cloud  
Create Grafana Cloud Account

Reference Documentation:  

    https://grafana.com/products/cloud/

# 1. Install Web Based Application on Ubuntu Server.   
Install it by following documentation.

    https://github.com/curioustushar/react-sample-projects 

    git clone https://github.com/curioustushar/react-sample-projects 
    sudo apt-get update
    sudo apt install npm

    npm i @grafana/faro-react
    npm i @grafana/faro-web-sdk
    cd  demo_cart
    npm install
    npm start

# 2. Configure the Application for Grafana Cloud.  
Add the below snippet in index.js under /src folder

    import { initializeFaro } from '@grafana/faro-web-sdk';

    const faro = initializeFaro({
    url: 'https://faro-collector-prod-us-east-0.grafana.net/collect/88fdcfc8e05a5c5534373b2ca505837e',
    apiKey: 'secret',
    app: {
        name: 'frontend',
        version: '1.0.0',
    },
    });   

Restart the application
    cd demo_cart
    npm start

# 3. Configure the Application for Tracing also.  

Install the below package for auto enablement of Tracing using Faro.

    npm i @grafana/faro-web-tracing

Modify the below snippet in index.js under /src folder

    import { getWebInstrumentations, initializeFaro } from '@grafana/faro-web-sdk';
    import { TracingInstrumentation } from '@grafana/faro-web-tracing';

    initializeFaro({
        url: 'https://faro-collector-prod-us-east-0.grafana.net/collect/88fdcfc8e05a5c5534373b2ca505837e',
        app: {
        name: 'frontend',
        version: '1.0.0',
        environment: 'production'
        },
        
        instrumentations: [
        // Mandatory, omits default instrumentations otherwise.
        ...getWebInstrumentations(),

        // Tracing package to get end-to-end visibility for HTTP requests.
        new TracingInstrumentation(),
        ],
    });

# 4. Verify the Web Vitals and RUM on Grafana Cloud

