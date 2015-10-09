# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.define "indigo"

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  config.vm.network "forwarded_port", guest: 80, host: 8080  # The web app
  config.vm.network "forwarded_port", guest: 9000, host: 9000  # The agent
  config.vm.network "forwarded_port", guest: 9042, host: 9042  # Cassandra native protocol

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  config.vm.hostname = "indigo"

  config.vm.provider "virtualbox" do |vb|
    vb.name = "Indigo"
    vb.memory = "2048"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "databases.yml"

    ansible.groups = {
      "indigo-databases" => ["indigo"],
      "indigo-webservers" => ["indigo"],
      "indigo:children" => ["indigo-databases", "indigo-webservers"]
    }

    # We override these variables to account for the default user being "vagrant" rather than "indigo".
    ansible.extra_vars = {
      ansible_ssh_user: "vagrant",
      install_dir: "/home/vagrant/"
    }
  end
end
