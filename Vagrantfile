# -*- mode: ruby -*-
# vi: set ft=ruby :
require "ipaddr"
base_ip="192.168.1.99"

Vagrant.configure("2") do |config|
  ip_address = IPAddr.new base_ip

	config.vm.provider "virtualbox" do |v|
	  v.memory = 1024
	end
  # Puertos para exponer en el Host

  config.vm.define "rancher_server" do |rserv|
      ip_address = ip_address.succ
      rserv.vm.network "forwarded_port", guest: 8080, host: 8080
      rserv.vm.network "forwarded_port", guest: 8443, host: 8443
      rserv.vm.network "forwarded_port", guest: 5000, host: 5000
      rserv.vm.box = "ubuntu/bionic64"
      rserv.vm.network "private_network", ip: ip_address.to_s
      # Instalando Docker
      rserv.vm.provision "docker" do |d|
        d.post_install_provision "shell", inline: "sudo cp /vagrant/daemon.json /etc/docker/;sudo service docker restart"
        # Instalando Rancher, Puertos para exponer en la maquina virtual
        d.run "rancher", image:"rancher/server",
          args: "-d --restart=unless-stopped -p 8080:8080 -p 8443:8443"
      end
   end


   config.vm.define "rancher_node" do |rnode|
       ip_address = ip_address.succ
       rnode.vm.box = "ubuntu/bionic64"
       rnode.vm.network "private_network", ip: ip_address.to_s
       # Instalando Docker
       rnode.vm.provision "docker" do |d|
         d.post_install_provision "shell", inline: "sudo cp /vagrant/daemon.json /etc/docker/;sudo service docker restart"
       end
    end

end
