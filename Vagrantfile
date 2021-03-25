Vagrant.configure("2") do |config|
# Define VMs as arrays, for easier configuration of the common params
# Separated into 2 arrays, for special handling of the master
  master = [
    {
      :name => "master",
      :eth1 => "192.168.205.10",
      :mem => "512",
      :cpu => "1"
    }
]

  slaves = [
    {
      :name => "slave1",
      :eth1 => "192.168.205.11",
      :mem => "512",
      :cpu => "1"
    },
    {
      :name => "slave2",
      :eth1 => "192.168.205.12",
      :mem => "512",
      :cpu => "1"
    }
]

  config.vm.box = "ubuntu/xenial64"
  config.vagrant.plugins = "vagrant-docker-compose"

# Run and provision the Slave VMs
slaves.each do |opts|
  config.vm.define opts[:name] do |config|
    config.vm.hostname = opts[:name]
    config.vm.provider "virtualbox" do |v|
      v.customize ["modifyvm", :id, "--name", opts[:name]]
      v.customize ["modifyvm", :id, "--memory", opts[:mem]]
      v.customize ["modifyvm", :id, "--cpus", opts[:cpu]]
    end
    config.vm.network :private_network, ip: opts[:eth1]
    config.vm.provision "shell", path: "shell-provisioners/install-ansible.sh"
    # Install Node Exporter via Ansible 
    config.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "Ansible/install_node_exporter.yaml"
      ansible.inventory_path = "Ansible/hosts"
      ansible.limit = 'all'
	end
  end
end

# Run and provision the Master VM
master.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.hostname = opts[:name]
      config.vm.synced_folder "./", "/vagrant", 
        owner: "vagrant",
        mount_options: ["dmode=775,fmode=600"]
      config.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--name", opts[:name]]
        v.customize ["modifyvm", :id, "--memory", opts[:mem]]
        v.customize ["modifyvm", :id, "--cpus", opts[:cpu]]
      end
      config.vm.network :private_network, ip: opts[:eth1]
      # Install Ansible on the VM Master via Shell
      config.vm.provision "shell", path: "shell-provisioners/install-ansible.sh"
      # Install Node Exporter via Ansible 
      config.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "Ansible/install_node_exporter.yaml"
        ansible.inventory_path = "Ansible/hosts"
        ansible.limit = 'all'
      end
      config.vm.provision :docker
      config.vm.provision :docker_compose, yml: "/vagrant/Docker/docker-compose.yaml", rebuild: true, run: "always"
      # docker run --rm -it -d --name prometheus -p 9090:9090 -v /opt/docker/prometheus:/etc/prometheus prom/prometheus --config.file=/etc/prometheus/prometheus.yml
    end
  end
end