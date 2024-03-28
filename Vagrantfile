Vagrant.configure("2") do |config|
  # Specify the base box
  config.vm.box = "bento/rockylinux-9"

  # Configure the provider to use Parallels
  config.vm.provider "parallels" do |prl|
    # Set the name of the VM in Parallels
    prl.name = "Grafana"

    # Set the amount of memory for the VM
    prl.memory = 2048

    # Set the number of CPUs for the VM
    prl.cpus = 2
  end

  # Configure network, synced folders, and other common settings here
  # For example, to set up a private network:
 # config.vm.network "private_network", type: "dhcp"
  config.vm.network "forwarded_port", guest: 3000, host: 3333
  config.vm.network "forwarded_port", guest: 9090, host: 3334

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "main.yml"
  end

end