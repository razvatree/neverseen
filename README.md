# neverseen
Demonstration purposes repo

Prerequisites: 
`Vagrant` installed on host machine
  
How to run:
1. Check out the repository
2. Run `vagrant up` inside the root directory of your local repository
  
Execution flow:
1. `Vagrant` spawns 3 VMs `master`, `slave1`, `slave2`
2. `Ansible` is installed via `shell provisioning`
3. `Ansible` installs `node_exporter` service on all machines
4. `Docker` is installed on master
5. `docker-compose` brings up `Grafana` and `Prometheus` containers
