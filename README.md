# Prometheus_Setup
Stack for Montoring and Alerting
Verify that Prometheus server was started via: http://localhost:9090 Check the status of scraping targets in Prometheus UI -> Status -> Targets

## What is Back-tier and Front-tier Networks in Docker-Compose.yml file ? ##
Networks are the layer that allow services to communicate with each other. The networking model exposed to a service is limited to a simple IP connection with target services and external resources, while the Network definition allows fine-tuning the actual implementation provided by the platform.

Networks can be created by specifying the network name under a top-level networks section. Services can connect to networks by specifying the network name under the service networks subsection

In the following example, at runtime, networks front-tier and back-tier will be created and the frontend service connected to the front-tier network and the back-tier network.

```
services:
  frontend:
    image: awesome/webapp
    networks:
      - front-tier
      - back-tier

networks:
  front-tier:
  back-tier:
  ```
The Grafana Dashboard is now accessible via: `http://<Host IP Address>:3000`
```
username - admin
password - hassan123 (Password is stored in the `/grafana/config.monitoring` env file)
```
You can also change password according to your desire . Just edit
```
GF_SECURITY_ADMIN_PASSWORD=<Your Password>
```
Verify the prometheus datasource configuration in Grafana. If it was not already configured, create a Grafana datasource with these settings:

`name: Prometheus(default)`
`type: prometheus`
`url: http://localhost:9090`
`access: browser`
# Grafana Dashboards 
## Node-Exporter Dashboard 
Nearly all default values exported by Prometheus node exporter graphed.

Only requires the default job_name: node, add as many targets as you need in ‘/etc/prometheus/prometheus.yml’.
```
  - job_name: 'node-exporter'
    static_configs:
      - targets: ['localhost:9100']
```      
Available on github: https://github.com/rfmoz/grafana-dashboards.git
You can Import dashboard through dashboard id which `1860` in this case or you can upload its json file available at this link https://grafana.com/api/dashboards/1860/revisions/30/download
