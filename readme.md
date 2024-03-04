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

##### Feb 22 2024:
I have sort of configured prometheus. need to change how the blockinline task formats the block (add 2 space indentation). 
I also need to adjust the perms for the /etc/prometheus-* directory and add a task in the playbook to start it automatically.
Next, need to figure out how to connect the prometheus service to the grafana server. There is some info about that [here](https://grafana.com/docs/grafana/latest/getting-started/get-started-grafana-prometheus/). 

##### Feb 23 2024:
Got Prometheus to sucessfully connect to our Uptime-Kuma server and it is scrapping data from the endpoints I monitor there. I used a community made dashboard for Uptime-kuma but 
it doesn't necessiarly have what I need. But it was nice to see it working. Still need to address the indention/formatting issue in the prometheus.yml file. This Grafana and Prometheus
instance are also only running on a Vagrant box so this project will need to be migrated to a production server eventually if we decide to keep using these products. There is also a Cloud hosted instance that might do all we need for free or very cheaply that I will investigate. 

##### Feb. 26th:
Had to rewrite part of the Vagrantfile config to use localhost instead of private ip addresses to work on campus network. Fixed issue with prometheus.yml file formatting. Need to add a task to actually start the prometheus service. Currently have to cd to prometheus directory and `sudo ./prometheus --config.file=./prometheus.yml` to start service. 

The actual grafana dashboards are not instantly intuitive to create. Will need to look into that more. 

##### Mar. 1st:
Started configuring how the playbook would work if it was also going to assign an SSL cert during the installation process. However, this is a bit more complicated then what I want to dive into right now. I decided to set those changes to a new branch and continue working on what needs to be done to deploy the app in it's current state to a live server. 

Those tasks are:
- [x] break out the prometheus set up to a separate playbook
- [x] create a prometheus user and group
- [x] change ownership of installed files and directories to that user and group
- [ ] create a servicefile for prometheus to work as a systemd unit 
- [ ] create a task or handler to make sure prometheus is started and enabled
- [ ] set project inventory so that it will actually target my live server (magpie)
- [ ] deploy


##### Mar. 4th: 
Updated task list:
- [x] create a servicefile for prometheus to work as a systemd unit 
- [x] create a task or handler to make sure prometheus is started and enabled
- [ ] deal with SELinux, which is preventing the systemd unit from starting 
- [ ] test to make sure everything is properly enabled and started between reboots
- [ ] set project inventory so that it will actually target my live server (magpie)
- [ ] deploy
