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

  server.vm.host_name = 'server'
  server.vm.network :private_network, ip: "192.168.56.7"

           server.vm.provision "shell", inline: <<-SHELL
       sudo yum install nfs-utils -y
       sudo mkdir /var/upload
       sudo chmod o+w /var/upload/
       sudo echo '/var/upload/ *(rw)' >> /etc/exports
       sudo exportfs -r
       sudo systemctl start firewalld
       sudo firewall-cmd --add-service=nfs 
       sudo firewall-cmd --add-protocol=udp
       sudo systemctl start nfs-server.service && systemctl enable nfs-server.service 
           SHELL


end


# Cent OS 8.2
# config used from this
# https://github.com/eoli3n/vagrant-pxe/blob/master/client/Vagrantfile
  config.vm.define "client" do |client|
    client.vm.host_name = 'client'
    client.vm.network :private_network, ip: "192.168.56.8"
    client.vm.provider :virtualbox do |vb|
      vb.memory = "1024"
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end
         client.vm.provision "shell", inline: <<-SHELL
     sudo yum install nfs-utils -y
     sudo mkdir /mnt/upload
     sudo mount -t nfs -o vers=3,proto=udp 192.168.56.7:/var/upload /mnt/upload/ 
     sudo grep upload /etc/mtab >> /etc/fstab
     touch /mnt/upload/file_from_client
         SHELL
        end
end
