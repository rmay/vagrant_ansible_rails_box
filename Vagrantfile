# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
  config.vm.box = 'bento/ubuntu-16.04'

  # Allow Vagrant to use my host's SSH keys
  config.ssh.forward_agent = true

  # To reach port 3000 from outside of Vagrant, use port 3001
  config.vm.network :forwarded_port, guest: 3000, host: 3001

  #=======================
  #= Syncing directories =
  #=======================

  # Option 1 (NSF):
  # Requires the following Rails config:
  #
  # `config.reload_classes_only_on_change = false`
  #
  # Setting it to `false` will reload classes on each requests in
  # case changes across network drives are not detected
  # ---------------------------------------------------------
  config.vm.synced_folder './projects', '/vagrant/projects', type: 'nfs'
  config.vm.network :private_network, ip: '192.168.50.4' # Required for NFS

  # Option 2 (Rsync):
  # Run `vagrant rsync-auto` after `vagrant up`
  # - or -
  # Install `vagrant plugin install vagrant-gatling-rsync`
  # ---------------------------------------------------------
  # config.vm.synced_folder '.', '/vagrant', type: 'rsync'

  config.vm.provider :virtualbox do |v|
    v.memory = 8192
    v.cpus = 2
    v.name = ENV['VIRTUAL_MACHINE_NAME']
  end

  config.vm.provision :ansible do |ansible|
    ansible.playbook = 'ansible/vagrant.yml'
    ansible.tags=ENV['TAGS'].split(',') if ENV['TAGS']
  end
end
