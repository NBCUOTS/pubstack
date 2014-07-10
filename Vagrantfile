# -*- mode: ruby -*-
# vi: set ft=ruby :

# Load local config:
require 'yaml'
config_file = File.dirname(__FILE__) + '/config.yml'
if not File.file?(config_file)
  raise Vagrant::Errors::VagrantError.new, 'You must create a config.yml file before using Vagrant.'
end
pubstack_config = YAML::load_file(config_file)

# Validate sites' docroots:
if pubstack_config.include? 'sites'
  pubstack_config['sites'].each {|site|
    documentroot = pubstack_config['synced_folder'] + '/' + site['vhost']['documentroot']
    if not File.directory?(File.expand_path(documentroot))
      raise Vagrant::Errors::VagrantError.new, 'The docroot ' + documentroot + ' does not exist.'
    end
  }
end

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = '2'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = 'pubstack'

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  config.vm.box_url = 'http://files.vagrantup.com/precise64.box'

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  config.ssh.forward_agent = true

  # Set up the config inside a block to allow for multiple-machines in
  # the future. This way the default box won't be lost when we update.
  config.vm.define 'dev' do |dev|
    # Create a private network, which allows host-only access to the machine
    # using a specific IP.
    dev.vm.network 'private_network', ip: '172.25.128.10'

    # Share an additional folder to the guest VM. The first argument is
    # the path on the host to the actual folder. The second argument is
    # the path on the guest to mount the folder. And the optional third
    # argument is a set of non-required options.
    dev.vm.synced_folder pubstack_config['synced_folder'], '/var/www/html'

    # Provider-specific configuration for VirtualBox:
    dev.vm.provider 'virtualbox' do |vb|
      vb.memory = pubstack_config['memory']
      vb.cpus = pubstack_config['cpus']
    end

    # Enable provisioning with Ansible.
    dev.vm.provision 'ansible' do |ansible|
      ansible.playbook = 'provisioning/pubstack.yml'
      ansible.extra_vars = {:sites => pubstack_config['sites']}
    end
  end
end
