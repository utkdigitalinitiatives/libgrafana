# Grafana and Uptime-Kuma integration

This is mostly a learning project meant to help me better understand:
1. monitoring tools like uptime-kuma and grafana
2. Ansible configuration management
3. More practice with Git version control

I will deploy grafana using Ansible as much as possible. First to a local Vagrant VM, then to a 
production server. I suspect some of the final configuration I will just need to do manually. 


### Current State:
I have Grafana installed and running. I have Prometheus installed. 
Next, need to configure Prometheus according to [this](https://medium.com/@tomer.klein/real-time-uptime-monitoring-with-uptime-kuma-and-grafana-16638d6a579f)

Feb 22 2024:
I have sort of configured prometheus. need to change how the blockinline task formats the block (add 2 space indentation). 
I also need to adjust the perms for the /etc/prometheus-* directory and add a task in the playbook to start it automatically.
Next, need to figure out how to connect the prometheus service to the grafana server. There is some info about that [here](https://grafana.com/docs/grafana/latest/getting-started/get-started-grafana-prometheus/). 
