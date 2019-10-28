# -*- mode: ruby -*-
# vi: set ft=ruby :

# No parallel provisioning so kmaster can initialize first before kworker
ENV['VAGRANT_NO_PARALLEL'] = 'yes'

Vagrant.configure(2) do |config|

  config.vm.provision "shell", path: "init.sh"

  # Kubernetes Master
  config.vm.define "kmaster" do |kmaster|
    kmaster.vm.box = "centos/7"
    kmaster.vm.hostname = "kmaster.domain.com"
    kmaster.vm.network "private_network", ip: "172.42.42.100"
    kmaster.vm.provider "virtualbox" do |v|
      v.name = "kmaster"
      v.memory = 2048
      v.cpus = 2
    end
    kmaster.vm.provision "shell", path: "init_kmaster.sh"
  end

  NodeCount = 2

  # Kubernetes Worker Nodes
  (1..NodeCount).each do |i|
    config.vm.define "kworker#{i}" do |workernode|
      workernode.vm.box = "centos/7"
      workernode.vm.hostname = "kworker#{i}.domain.com"
      workernode.vm.network "private_network", ip: "172.42.42.10#{i}"
      workernode.vm.provider "virtualbox" do |v|
        v.name = "kworker#{i}"
        v.memory = 1024
        v.cpus = 1
      end
      workernode.vm.provision "shell", path: "init_kworker.sh"
    end
  end

end