## To Clone this Branch use this command

`git clone -b loki --single-branch https://github.com/Muhammadhassan1226/Prometheus_Setup.git`


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


# Grafana Loki 

`name: loki(default)`
`url: http://loki:3100`
`access: browser`

## Docker Driver Client
Grafana Loki officially supports a Docker plugin that will read logs from Docker containers and ship them to Loki. The plugin can be configured to send the logs to a private Loki instance

## Installing 
The Docker plugin must be installed on each Docker host that will be running containers you want to collect logs from.

Run the following command to install the plugin:

``` docker plugin install grafana/loki-docker-driver:latest --alias loki --grant-all-permissions ```

To check installed plugins, use the docker plugin ls command. Plugins that have started successfully are listed as enabled:

``` $ docker plugin ls
ID                  NAME         DESCRIPTION           ENABLED  
ac720b8fcfdb        loki         Loki Logging Driver   true

```
## Change the logging driver for a container

Goto `/etc/docker/daemon.json` and edit this

```
{
    "log-driver": "loki",
    "log-opts": {
        "loki-url": "http://localhost:3100/loki/api/v1/push",
        "loki-batch-size": "400"
    }
}

```
After that restart docker service by using this command `sudo systemctl restart docker`
If the daemon.json is left as default, to specify the docker logging driver:
```
docker run --log-driver=loki \
    --log-opt loki-url="https://<user_id>:<password>@logs-us-west1.grafana.net/loki/api/v1/push" \
    --log-opt loki-retries=5 \
    --log-opt loki-batch-size=400 \
    grafana/grafana
    
```
