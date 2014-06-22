# -*- mode: ruby -*-
# vi: set ft=ruby :

# Load local config:
require "yaml"
config_file = File.dirname(__FILE__) + "/config.yml";
if not File.file?(config_file)
  raise 'You must create a config.yml file before using Vagrant.'
end
pubstack_config = YAML::load_file(config_file)

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "pubstack"

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine.
  # If you want to forward ports just follow the example in default.config.yml
  if not pubstack_config["virtualbox"]["port-forwarding"].nil?
    pubstack_config["virtualbox"]["port-forwarding"].each do |port|
      config.vm.network "forwarded_port", guest: port['guest'], host: port['host']
    end
  end

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: pubstack_config["vagrant"]["ip"]

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  config.ssh.forward_agent = true

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder pubstack_config["vagrant"]["synced_folder"], "/var/www/html"

  # Provider-specific configuration for VirtualBox:
  config.vm.provider "virtualbox" do |vb|
    # Use VBoxManage to customize the VM. For example to change memory:
    vb.customize ["modifyvm", :id, "--memory", pubstack_config["virtualbox"]["memory"]]
  end

  # Enable provisioning with Ansible.
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/pubstack.yml"
    ansible.extra_vars = pubstack_config["ansible"]
  end
end
