Vagrant.configure("2") do |config|
  config.vm.box = "generic/centos8"
  config.vm.network :private_network, ip: "192.168.56.66"
  config.vm.hostname = "grafana.test"
  config.ssh.insert_key = false

  config.vm.provider :virtualbox do |v|
    v.memory = 2048
    v.cpus = 2
  end

 # Provisioning configuration for Ansible
config.vm.provision "ansible" do |ansible|
  ansible.playbook = "playbook.yml"
end

end
