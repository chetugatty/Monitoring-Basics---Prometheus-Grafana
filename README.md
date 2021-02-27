# Monitoring-Basics
# Prometheus and Grafana

The following proceedure describe installation instruction for node_exporter, Prometheus and Grafana

## Node_exporter
	
  1. Download tar file and untar on same working path.

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.0.1/node_exporter-1.0.1.linux-amd64.tar.gz
tar -xvzf node_exporter-1.0.1.linux-amd64.tar.gz
```
	
  2. Check the content of node_exporter-1.0.1.linux-amd64 directory
  
  3. Create and enable service to startup during system boot process, Start the service and check the status.
  refer node_exporter.service project file for creating service inside /etc/systemd/system
  
```bash
systemctl enable node_exporter
systemctl start node_exporter
systemctl status node_exporter
```
  4. Check if the port is consumed using
  
```bash
netstat -lntu | grep 9100
```
  5. Now you have configured Node_exporter service successfully and validation is completed
  
  6. Validate the metrics : http://localhost:9100/metrics

  7. Go through documentation on metrics exported by node_exporter : https://github.com/prometheus/node_exporter

 
## Prometheus

1. Download tar file and untar on same working path.

```bash
wget https://github.com/prometheus/prometheus/releases/download/v2.19.2/prometheus-2.19.2.linux-amd64.tar.gz
tar -xvzf prometheus-2.19.2.linux-amd64.tar.gz
```
	
  2. Check the content of prometheus-2.19.2.linux-amd64.tar.gz directory
  
  3. The Prometheus server is a single binary called prometheus (or prometheus.exe on Microsoft Windows). We can run the binary and see help on its options by passing the --help flag.
  
  4. Before starting Prometheus, let's configure it. Prometheus configuration is YAML. The Prometheus download comes with a sample configuration in a file called prometheus.yml that is a good place to get started.
  
  5. Update prometheus.yml with node_exporter details (hostname/IP:port_number). Create new job inside "scrape_configs".
  
  6. Use promtool to validate the configuration : ./promtool check config prometheus.yml
  
  7. Create and enable service to startup during system boot process, Start the service and check the status.
  refer prometheus.service project file for creating service inside /etc/systemd/system
  
```bash
systemctl enable prometheus
systemctl start prometheus
systemctl status prometheus
```
  8. Check if the port is consumed using
  
```bash
netstat -lntu | grep 9090
```
  9. scrape_configs, controls what resources Prometheus monitors. Since Prometheus also exposes data about itself as an HTTP endpoint it can scrape and monitor its own health. In the default configuration there is a single job, called prometheus, which scrapes the time series data exposed by the Prometheus server. The job contains a single, statically configured, target, the localhost on port 9090. Prometheus expects metrics to be available on targets on a path of /metrics. So this default job is scraping via the URL: http://localhost:9090/metrics.
  
  10. Validate the metrics through WebUI : http://<server_FQDN>:9090/graph
  
  11. Check Targets and its status, Execute some query to check metrics
  
```bash  
node_memory_MemFree_bytes
node_cpu_seconds_total
node_filesystem_avail_bytes
```
  
  12. Now you have configured Node_exporter service successfully and validation is completed

  13. Go through documentation on for more information on Prometheus : https://prometheus.io/docs/prometheus/latest/getting_started/

## Grafana

1. Download Grafana tar file and execute following command

```bash
wget https://dl.grafana.com/oss/release/grafana-7.0.6-1.x86_64.rpm
sudo rpm -i --nodeps grafana-7.0.6-1.x86_64.rpm
```
	
  2. Follow the instraurction from above command enable service and start Grafana, Following are sample instructions from console
 ```bash
warning: grafana-7.0.6-1.x86_64.rpm: Header V4 RSA/SHA256 Signature, key ID 24098cb6: NOKEY
### NOT starting on installation, please execute the following statements to configure grafana to start automatically using systemd
 sudo /bin/systemctl daemon-reload
 sudo /bin/systemctl enable grafana-server.service
### You can start grafana-server by executing
 sudo /bin/systemctl start grafana-server.service
``` 
  3. Open your web browser and go to http://localhost:3000/. 3000 is the default HTTP port that Grafana listens to if you haven’t configured a different port. Provide "admin" as both username and password, Change password or skip to continue.
  
  4. Check various Menu options, plugins, Datasources available. Go through documentaion https://grafana.com/docs/grafana/latest/getting-started/getting-started/ for more details.
  
  5. Select "Configuration", Datasources and select "Add Data source". Under Time series Datasource, select "Prometheus". Update the required details about previous prometheus configuration and select "Save & Test" to create the datasource
  
 ```bash
Http URL : http://localhost:9090
```   
    
  6. Select any opensouorce available dashboard from "https://grafana.com/grafana/dashboards?category=databases" and download the json file. Otherwise search for "Node Exporter for Prometheus Dashboard English Compatibility Version" and download.
  
  7. In Grafana, Select "+" and "Import" option. Select the dowloaded dashboard Json file and import. Provide required Name and existing datasource details.
  
  8. Enjoy working with Grafana :)
