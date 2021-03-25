# neverseen
Demonstration purposes repo

Prerequisites: 
  -Vagrant installed on host machine
  
How to run:
  -Check out the repository
  -Run "vagrant up" inside the root directory of your local repository
  
This will call Vagrant to spawn up 3 VMs, labeled "master", "slave1", "slave2".
Ansible will be installed on each machine via shell provisioning
Node_exporter service will be installed via Ansible.
Docker will be installed on the "master" VM
Docker-compsose will be called to spawn the Grafana and Prometheus services on the master
  
