# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

auto = true
# Increase numworkers if you want more than 3 nodes
numworkers = 2

# VirtualBox settings
# Increase vmmemory if you want more than 512mb memory in the vm's
vmmemory = 512
# Increase numcpu if you want more cpu's per vm
numcpu = 1

instances = []

(1..numworkers).each do |n| 
  instances.push({:name => "worker#{n}", :ip => "10.2.2.#{n+2}"})
end

manager_ip = "10.2.2.2"

File.open("./hosts", 'w') { |file| 
  instances.each do |i|
    file.write("#{i[:ip]} #{i[:name]} #{i[:name]}\n")
  end
  file.write("#{manager_ip} manager manager\n")
}

# Vagrant version requirement
Vagrant.require_version ">= 1.8.4"

Vagrant.configure("2") do |config|
    config.vm.provider "virtualbox" do |v|
     	v.memory = vmmemory
  	v.cpus = numcpu
    end
    
    config.vm.define "manager" do |i|
      i.vm.box = "ubuntu/trusty64"
      i.vm.hostname = "manager"
      i.vm.network "private_network", ip: "#{manager_ip}"
      # Proxy
      i.vm.provision "shell", path: "./provision.sh"
      if File.file?("./hosts") 
        i.vm.provision "file", source: "hosts", destination: "/tmp/hosts"
        i.vm.provision "shell", inline: "cat /tmp/hosts >> /etc/hosts", privileged: true
      end 
      if auto 
        i.vm.provision "shell", inline: "docker swarm init --advertise-addr #{manager_ip}"
        i.vm.provision "shell", inline: "docker swarm join-token -q worker > /vagrant/token"
      end
	  i.vm.provision "shell", inline: "mkdir -p /mnt/registry", privileged: true
	  i.vm.provision "shell", inline: "docker node update --label-add registry=true manager"
	  i.vm.provision "shell", inline: "docker service create --name registry --constraint 'node.labels.registry==true' --mount type=bind,src=/mnt/registry,dst=/var/lib/registry -e REGISTRY_HTTP_ADDR=0.0.0.0:5000 --publish published=5000,target=5000 --replicas 1 registry:2"
    end 

  instances.each do |instance| 
    config.vm.define instance[:name] do |i|
      i.vm.box = "ubuntu/trusty64"
      i.vm.hostname = instance[:name]
      i.vm.network "private_network", ip: "#{instance[:ip]}"
      # Proxy
      i.vm.provision "shell", path: "./provision.sh"
      if File.file?("./hosts") 
        i.vm.provision "file", source: "hosts", destination: "/tmp/hosts"
        i.vm.provision "shell", inline: "cat /tmp/hosts >> /etc/hosts", privileged: true
      end 
      if auto
        i.vm.provision "shell", inline: "docker swarm join --advertise-addr #{instance[:ip]} --listen-addr #{instance[:ip]}:2377 --token `cat /vagrant/token` #{manager_ip}:2377"
      end
    end 
  end
end
