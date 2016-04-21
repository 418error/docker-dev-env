# -*- mode: ruby -*-
# vi: set ft=ruby :

[
  {:name => "vagrant-cachier", :version => "1.2.1"},
  {:name => "vagrant-git", :version => "0.1.7"},
  {:name => "vagrant-docker-compose", :version => "1.0.0"}
].each do |plugin|
  if not Vagrant.has_plugin?(plugin[:name], plugin[:version])
    raise "#{plugin[:name]} #{plugin[:version]} is required. Please run `vagrant plugin install #{plugin[:name]}`"
  end
end


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
