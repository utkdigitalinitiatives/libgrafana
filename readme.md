
# Purpose
The primary purpose of this project was to implement a Zabbix replacement. The secondary objectives of this project was to gain experience deploying and configuring servers via Ansible. 

## Some Terms:
### Grafana:
Grafana is an open-source and interactive visualization web application. It provides charts, graphs, and alerts for the web when connected to supported data sources. Primarily, it is used to visualize time series data for infrastructure and application analytics but also supports other graph types. 

### Prometheus:
Prometheus is an open-source monitoring and alerting toolkit. It has a powerful query language, time series database, and simple integration with a wide range of systems and services. Prometheus primarily collects and store metrics as time series data, i.e., data with timestamps. 

### Uptime-Kuma: 
Uptime-Kuma is a clone of UptimeRobot, which is a monitoring tool that provides status checks for various types of services and websites. 

## Summary of Project
This project uses Prometheus to connect to a previously configured Uptime-Kuma instance to scrape data in order to use in a Grafana visualization. Future goals of the project would be use use Prometheus' Node Exporter to collect and visualize additional data about our servers and other resources. 