Vagrant.configure("2") do |config|
  config.vm.box = "generic/centos8"
#  config.vm.network :private_network, ip: "192.168.56.66"
  config.vm.network "forwarded_port", guest: 3000, host: 3333
  config.vm.network "forwarded_port", guest: 9090, host: 3334
  config.vm.hostname = "grafana.test"
  config.ssh.insert_key = false

  config.vm.provider :virtualbox do |v|
    v.memory = 2048
    v.cpus = 2
  config.vm.synced_folder ".", "/vagrant"


  end

 # Provisioning configuration for Ansible
config.vm.provision "ansible" do |ansible|
  ansible.playbook = "main.yml"
end

end
