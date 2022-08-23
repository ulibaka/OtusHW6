# -*- mode: ruby -*-
# vi: set ft=ruby :
VAGRANT_EXPERIMENTAL="disks"

Vagrant.configure("2") do |config|

  config.vm.box = 'centos/7'
#  config.vm.box_url = 'https://cloud.centos.org/centos/8/x86_64/images/CentOS-8-Vagrant-8.4.2105-20210603.0.x86_64.vagrant-virtualbox.box'
#  config.vm.box_download_checksum = 'dfe4a34e59eb3056a6fe67625454c3607cbc52ae941aeba0498c29ee7cb9ac22'
#  config.vm.box_download_checksum_type = 'sha256'

config.vm.define "server" do |server|
config.vm.synced_folder ".", "/vagrant", disabled: true
  server.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  server.vm.disk :disk, size: "1GB", name: "disk1"
  server.vm.disk :disk, size: "1GB", name: "disk2"
  server.vm.disk :disk, size: "1GB", name: "disk3"
  server.vm.host_name = 'server'
  server.vm.network :private_network, ip: "192.168.56.7"
  server.vm.provision "shell",

    name: "Setup zfs",
    path: "setup_zfs.sh"
  end


# Cent OS 8.2
# config used from this
# https://github.com/eoli3n/vagrant-pxe/blob/master/client/Vagrantfile
  config.vm.define "client" do |client|
    client.vm.host_name = 'client'
    client.vm.network :private_network, ip: "192.168.56.8"
    client.vm.disk :disk, size: "1GB", name: "disk4"
    client.vm.disk :disk, size: "1GB", name: "disk5"
    client.vm.disk :disk, size: "1GB", name: "disk6"
    client.vm.provider :virtualbox do |vb|
      vb.memory = "1024"
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end
    client.vm.provision "shell",
    name: "Setup zfs",
    path: "setup_zfs.sh"
  end
end
