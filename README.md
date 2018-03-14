# vagrant-docker-swarm
Small example of building a docker swarm for testing using vagrant. 

# Configuration

Set numworkers for more virtual machines, change vmmemory and numcpu for more resources for
each virtual machine in the Vagrantfile.

```ruby
numworkers = 2
vmmemory = 512
numcpu = 1
``` 

The docker swarm is built automatically by sharing the worker token via a shared file on
the host machine.

A private network starting at 10.2.2.2 (manager node) is created and incremented by one
for each worker. 

# Usage

Start the virtual machines

```bash
vagrant up
``` 


SSH into the different machines with their names:
```bash
vagrant ssh manager
vagrant ssh worker1
vagrant ssh worker2
``` 

If you ssh into the manager node you can launch docker services:

```bash
docker service create --replicas 3 --name webserver -p 80:80 nginx
``` 

On the host system open a browser and enter the IP of one of the machines
of your swarm 10.2.2.2, 10.2.2.3, 10.2.2.4 and you should see the welcome
message of nginx.

If you want to remove the webserver just execute:

```bash
docker service rm webserver
``` 

To destroy everything again

```bash
vagrant destroy -f
``` 


# Inspiration

Inspired by: https://github.com/tdi/vagrant-docker-swarm



