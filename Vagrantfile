# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version. Please don't change it unless you know what you're doing.
Vagrant.configure("2") do |config|
  # Use the geerlingguy/ubuntu2004 box because it has been recommended for use in the IP instructions.
  config.vm.box = "geerlingguy/ubuntu2004"

  # Prevent Vagrant from automatically replacing the default SSH key with a unique one.
  # Setting this to false ensures the default "insecure" key is used for all VMs, simplifying Ansible access.
  config.ssh.insert_key = false
  
  config.vm.provision "shell", inline: <<-SHELL
    # Update the package list to ensure we have the latest information on available packages
    sudo apt-get update
    
    # Install required packages:
    # - ca-certificates: provides trusted certificates for secure connections
    # - ansible: configuration management and automation tool
    # - docker.io: Docker engine to run containerized applications
    sudo apt-get install -y ca-certificates ansible docker.io
    
    # Add the 'vagrant' user to the 'docker' group, allowing Docker commands to run without sudo
    sudo usermod -aG docker vagrant    
  SHELL  

end
