# -*- mode: ruby -*-
# vi: set ft=ruby :

# Boostrap Script
$script = <<SCRIPT

systemctl stop update-engine

docker build -t turistforeningen/redis - < /home/core/vagrant/containers/redis.docker
docker build -t turistforeningen/mongo - < /home/core/vagrant/containers/mongo.docker

cp /home/core/vagrant/units/*.service /media/state/units/.
systemctl restart local-enable.service

SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "coreos"
  config.vm.box_url = "http://storage.core-os.net/coreos/amd64-generic/dev-channel/coreos_production_vagrant.box"

  config.vm.network :private_network, ip: "172.12.8.150"
  config.vm.synced_folder "~/Developer", "/srv/containers", id: "core", :nfs => true,  :mount_options   => ['nolock,vers=3,udp']
  config.vm.synced_folder ".", "/home/core/vagrant", id: "core", :nfs => true,  :mount_options   => ['nolock,vers=3,udp']

  config.vm.provision :shell, :inline => $script

  # Fix docker not being able to resolve private registry in VirtualBox
  config.vm.provider :virtualbox do |vb, override|
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end
end
