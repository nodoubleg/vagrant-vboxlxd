# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  onfig.vm.box = "ubuntu/xenial64"

  config.vm.provider "virtualbox" do |vb|
    # Do this as a linked clone if we can.
    vb.linked_clone = true if Gem::Version.new(Vagrant::VERSION) >= Gem::Version.new('1.8.0')
    # Containers are light, but we should still have some resources.
    vb.memory = 4096
    # At least 2 CPUs
    vb.cpus = 2
    vagrant_root = File.dirname(File.expand_path(__FILE__))
    file_to_disk = File.join(vagrant_root, 'lxd_zpool.vdi')
    unless File.exist?(file_to_disk)
      # lxc images are tiny by default. Not much storage needed.
      vb.customize ['createhd', '--filename', file_to_disk, '--size', 5 * 1024]
    end
    # enabling hostiocache like this isn't safe, but it should be faster.
    vb.customize ['storagectl', :id, '--name', 'SCSI', '--hostiocache', 'on']
    vb.customize ['storageattach', :id, '--storagectl', 'SCSI', '--port', 2, '--device', 0, '--type', 'hdd', '--medium', file_to_disk]
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y zfs
    zpool create -f lxd /dev/sdc
    lxd init --auto --storage-backend=zfs --storage-pool=lxd
    cp /vagrant/scripts/* /usr/local/bin/
    chmod a+rx /usr/local/bin/*
    # If we keep this VM around, let's keep images up to date but not nuke them so fast.
    lxc config set images.remote_cache_expiry 30
    lxc config set images.auto_update_interval 24
    lxc config set images.auto_update_cached true
    # Start out by preloading/caching some container images
    lxc launch -e images:centos/7 preload
    lxc delete --force preload
    lxc launch -e images:centos/6 preload
    lxc delete --force preload
    lxc launch -e images:ubuntu/xenial preload
    lxc delete --force preload
  SHELL
end
