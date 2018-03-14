# vagrant-docker-swarm
Small example of building a docker swarm for testing using vagrant. 

#Configuration

Set numworkers for more virtual machines, change vmmemory and numcpu for more resources for
each virtual machine.

```ruby
numworkers = 2
vmmemory = 512
numcpu = 1
``` 

The docker swarm is built automatically by sharing the worker token via a shared file on
the host machine.

SSH into the different machines with their names:
vagrant ssh manager
vagrant ssh worker1
vagrant ssh worker2

#Inspiration

Inspired by: https://github.com/tdi/vagrant-docker-swarm



