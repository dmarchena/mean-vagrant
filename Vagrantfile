# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.require_version ">= 1.1"

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  config.vm.define :mongodb do |mongodb|
    mongodb.vm.box = "ubuntu_precise64"
    mongodb.vm.box_url = "http://files.vagrantup.com/precise64.box"
    mongodb.vm.network "private_network", ip: "192.168.56.11"

    # This shell provisioner installs librarian-puppet and runs it to install
    # puppet modules. This has to be done before the puppet provisioning so that
    # the modules are available when puppet tries to parse its manifests.
    # must pass in directory of the Puppetfile location
    mongodb.vm.provision "shell", 
      path: "shell/librarian-puppet.sh", 
      args: "puppet/librarian/mongodb"

    # Now run the puppet provisioner. Note that the modules directory is entirely
    # managed by librarian-puppet
    mongodb.vm.provision "puppet" do |puppet|
      puppet.manifests_path = "puppet/manifests/mongodb"
      puppet.manifest_file = "default.pp"
    end

    config.vm.post_up_message = "mongodb listening on 192.168.56.11"
  end

  config.vm.define :appserver do |appserver|
    appserver.vm.box = "ubuntu_precise64"
    appserver.vm.box_url = "http://files.vagrantup.com/precise64.box"
    appserver.vm.network "private_network", ip: "192.168.56.12"
    appserver.vm.network "forwarded_port", guest: 3000, host: 3000
    
    appserver.vm.synced_folder "../workspace", "/usr/local/src/mean", create: true

    # This shell provisioner installs librarian-puppet and runs it to install
    # puppet modules. This has to be done before the puppet provisioning so that
    # the modules are available when puppet tries to parse its manifests.
    # must pass in directory of the Puppetfile location
    appserver.vm.provision "shell", 
      path: "shell/librarian-puppet.sh", 
      args: "puppet/librarian/appserver"

    # Now run the puppet provisioner. Note that the modules directory is entirely
    # managed by librarian-puppet
    appserver.vm.provision "puppet" do |puppet|
      puppet.manifests_path = "puppet/manifests/appserver"
      puppet.manifest_file = "default.pp"
    end

    config.vm.post_up_message = "appserver listening on 192.168.56.12"
  end
end
