Vagrant.configure("2") do |config|
  config.vm.box = "debian/bullseye64"

  config.vm.provider "virtualbox" do |v|
    v.memory = 256
    v.cpus = 1
  end

  config.vm.synced_folder ".", "/vagrant", disabled: true

  $cliente_ansible = <<-SCRIPT
    cat /vagrant/id_rsa.pub >> .ssh/authorized_keys
  SCRIPT

  $servidor_ansible = <<-SCRIPT
    apt-get update
    apt-get install -y ansible
    cp /vagrant/chaves/id_rsa_* /home/vagrant/.ssh/
    chmod 600 /home/vagrant/.ssh/id_rsa_*
    chown vagrant:vagrant /home/vagrant/.ssh/id_rsa_*
  SCRIPT

  config.vm.define "pote" do |manager1|
    manager1.vm.hostname = "pote.example.com"
    manager1.vm.synced_folder "./pote", "/vagrant"
    manager1.vm.network "private_network", ip: "172.16.10.254", virtualbox__intnet: "swarm"
    manager1.vm.provision "shell", inline: $servidor_ansible
  end

  config.vm.define "sputnik" do |worker1|
    worker1.vm.hostname = "worker1.example.com"
    worker1.vm.synced_folder "./workers/worker1", "/vagrant"
    worker1.vm.network "private_network", ip: "172.16.10.1", virtualbox__intnet: "swarm"
    worker1.vm.provision "shell", inline: $cliente_ansible
  end

  config.vm.define "rambo" do |worker2|
    worker2.vm.hostname = "worker2.example.com"
    worker2.vm.synced_folder "./workers/worker2", "/vagrant"
    worker2.vm.network "private_network", ip: "172.16.10.2", virtualbox__intnet: "swarm"
    worker2.vm.provision "shell", inline: $cliente_ansible
  end

  config.vm.define "mila" do |worker3|
    worker3.vm.hostname = "worker3.example.com"
    worker3.vm.synced_folder "./workers/worker3", "/vagrant"
    worker3.vm.network "private_network", ip: "172.16.10.3", virtualbox__intnet: "swarm"
    worker3.vm.provision "shell", inline: $cliente_ansible
  end

end
