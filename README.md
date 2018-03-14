# vagrant-docker-swarm
Small example of building a docker swarm for testing using vagrant. 

Set numworkers for more virtual machines, change vmmemory and numcpu for more resources for
each virtual machine. 

The docker swarm is built automatically.

SSH into the different machines with their names:
vagrant ssh manager
vagrant ssh worker1
vagrant ssh worker2

Inspired by: https://github.com/tdi/vagrant-docker-swarm



