# -*- mode: ruby -*-
# vi: set ft=ruby :

$lxd = <<-LXD
sudo apt-get update -y
sudo snap install lxd
sudo gpasswd -a vagrant lxd
exit
LXD

$for_port = <<-PFW
echo "configuracion de puerto al contenedor haproxy"
lxc config device add haproxy http proxy listen=tcp:0.0.0.0:5080 connect=tcp:127.0.0.1:80
PFW

Vagrant.configure("2") do |config|

  if Vagrant.has_plugin? "vagrant-vbguest"
    config.vbguest.no_install  = true
    config.vbguest.auto_update = false
    config.vbguest.no_remote   = true
  end

  config.vm.define :balanceadora do |balanceadora|
    balanceadora.vm.box = "ubuntu/bionic64"
    balanceadora.vm.network :private_network, ip: "192.168.100.7"
    balanceadora.vm.hostname = "balanceadora"
    balanceadora.vm.provision :shell, inline: $lxd
    #balanceadora.vm.provision :shell, path: "certi_cluster.sh"
    balanceadora.vm.provision :shell, path: "script.sh"
    balanceadora.vm.provision :shell, path: "copy_index.sh"
    balanceadora.vm.provision :shell, path: "conf_haproxy.sh"
	balanceadora.vm.provision :shell, inline: $for_port
  end

  config.vm.define :servers1 do |servers1|
    servers1.vm.box = "ubuntu/bionic64"
    servers1.vm.network :private_network, ip: "192.168.100.8" 
    servers1.vm.hostname = "servers1"
    servers1.vm.provision :shell, inline: $lxd
    #servers1.vm.provision :shell, path: "join_cluster1.sh"
  end

  config.vm.define :servers2 do |servers2|
    servers2.vm.box = "ubuntu/bionic64"
    servers2.vm.network :private_network, ip: "192.168.100.9" 
    servers2.vm.hostname = "servers2"
    servers2.vm.provision :shell, inline: $lxd
    #servers2.vm.provision :shell, path: "join_cluster2.sh"
  end

end
