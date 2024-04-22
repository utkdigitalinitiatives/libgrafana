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



##### Mar. 4th: 
Created servicefile for prometheus and set some handlers for it. 

##### Mar 18th: 
Re-organized project structure so that main.yml calls separate playbooks to install and configure Grafana and Prometheus. I was having an issue going from the main playbook to the prometheus one and back. The solutions to my issue were to either rewrite the prometheus playbook to be just a series of tasks, or more simply, create a new main playbook that simply imports the other two playbooks. This will also help going forward if I need to create another playbook. For instance, I might create a new playbook to set up the linux server environment (SELinux, users and groups, Firewall settings, Apache settings, etc) before starting the app installation process. In the future, this set up playbook could have conditional logic to work with any flavor or linux or webserver technology. I will likely need another playbook to configure SSL certification stuff as well (this might be a geerlingguy role, I am not 100% sure.). 

##### Mar 21st:
Big re-write of the Prometheus playbook. I resolved my SELinux issues by more closely adhering to Linux Filesystem Hierarchy. SELinux did not like, for example, application state changed being written to `/etc/` so I moved that to `/var/lib/`. Also moved the binaries to `/usr/local/bin/`. 

Also started writing the apache conf file that this app will need. I need to decide how that part will actually be deployed. I will probably create a new playbook just for the apache config. 

After that I need to figure out how to handle the ssl cert. I am hoping to figure out the geerlingguy.certbot role for that part. I am just worried about it somehow messing up the existing vhosts on the server. 

##### Mar 28th:
Due to getting a new work laptop, I had to spend some time back up my test environments. There was a few complications in going from Intel Mac to M3 chip. The big thing for this project is that the prometheus-setup.yml playbook now has some conditional logic to detect which cpu architecture the host is using and it will download and install the appropriate version. Between this and other work I was not able to make much progress on the apache/ssl cert side of things this week. 

##### April 18th:
Sucessuflly deployed to magpie on the 17th (yesterday). The major issues during deployment were:
- change certbot role variables in the actual role. Ex. need to change certbot_create_standalone_stop_services from 'apache' to 'httpd' to reflect the RHEL environment. I think I can just add this as a varible in vars.yml but i'm not sure. 
- In the inventory file, leave out the www on the hostname. 
- The apache conf file needs the server ip in the `<VirtualHost *:80>` block. 
- Minor syntax errors throughout. ex. `ErrorLog ${APACHE_LOG_DIR}/` instead of `ErrorLog {{ APACHE_LOG_DIR }}/`

Updated task list:
- [x] create a servicefile for prometheus to work as a systemd unit 
- [x] create a task or handler to make sure prometheus is started and enabled
- [x] deal with SELinux, which is preventing the systemd unit from starting 
- [x] test to make sure everything is properly enabled and started between reboots
- [x] create playbook to deploy apache config
- [x] Figure out how to handle ssl cert/certbot
- [x] set project inventory so that it will actually target my live server (magpie)
- [x] deploy

