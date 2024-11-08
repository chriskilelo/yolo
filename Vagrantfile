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

  # Forward port 80 on the VM to port 8080 on the host (for the frontend application)
  config.vm.network "forwarded_port", guest: 80, host: 8080  
  # Forward port 5000 on the VM to port 1234 on the host (for the backend application)
  config.vm.network "forwarded_port", guest: 5000, host: 1234 
  # Forward MongoDB's default port 27017 on the VM to port 27017 on the host
  config.vm.network "forwarded_port", guest: 27017, host: 27017

  # Sync the project directory on the host to the /vagrant directory in the VM
  # This allows access to code and project files within the VM
  config.vm.synced_folder ".", "/vagrant"

  # Provisioning configuration for Ansible.
  config.vm.provision "ansible" do |ansible|
    # Specify the main Ansible playbook to run
    ansible.playbook = "tasklist.yaml"
    # Specify the inventory file if needed (list of hosts for Ansible to manage)
    ansible.inventory_path = "inventory"
    # Define extra variables to pass to Ansible (e.g., the Python interpreter to use)
    ansible.extra_vars = {
      ansible_python_interpreter: "/usr/bin/python3"  # Ensure Python 3 is used by Ansible
    }
  end  

end
