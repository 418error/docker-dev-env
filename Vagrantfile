# -*- mode: ruby -*-
# vi: set ft=ruby :

# Install vagrant plugin
#
# @param: plugin type: Array[String] desc: The desired plugin to install
def install_plugins(plugins)
  logger = Vagrant::UI::Colored.new
  result = false
  plugins.each do |p|
    pm = Vagrant::Plugin::Manager.new(
      Vagrant::Plugin::Manager.user_plugins_file
    )
    plugin_hash = pm.installed_plugins
    next if plugin_hash.has_key?(p)
    result = true
    logger.warn("Installing plugin #{p}")
    pm.install_plugin(p)
  end
  if result
    logger.warn('Re-run vagrant up now that plugins are installed')
    exit
  end
end

project_plugins = ["vagrant-cachier","vagrant-git","vagrant-docker-compose"]

install_plugins(project_plugins)

Vagrant.configure(2) do |config|
  config.vm.box              = "centos/7"
  config.vm.box_version      = "1603.01"
  config.vm.box_check_update = false
  config.ssh.forward_agent = true

  config.vm.provider :virtualbox do |vb|
    # TODO Make this box unique, add the dev env name?
    vb.name = "channie-solutions-development-#{Time.now.to_f}"
    vb.customize ['modifyvm', :id, '--memory', ENV['VM_MEMORY'] || 4096]
    vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
    vb.customize ['modifyvm', :id, '--natdnsproxy1', 'on']
    vb.customize ["modifyvm", :id, "--cpus", ENV['VM_CPUS'] || 4]
    vb.customize ['modifyvm', :id, '--paravirtprovider', 'kvm']
  end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   sudo apt-get update
  #   sudo apt-get install -y apache2
  # SHELL
end
