# -*- mode: ruby -*-
# vi: set ft=ruby :

NODES = {
  "manager1" => "192.168.99.100",
  "worker1" => "192.168.99.101",
  "worker2" => "192.168.99.102",
}

Vagrant.configure("2") do |config|
  NODES.each do |(node_name, ip_address)|
    config.vm.define node_name do |node|
      node.vm.box = "bento/ubuntu-20.04"
      node.vm.hostname = node_name
      node.vm.network "private_network", ip: ip_address

      node.vm.provider "virtualbox" do |v|
        v.name = node_name
        v.memory = "1024"
        v.cpus = "1"
      end

      node.vm.provision "shell", inline: <<-SHELL
        # Faire en sorte que les machines puissent communiquer entre elles via leur hostnames (exemple: ping worker1 depuis manager1)
        #{NODES.map{ |n_name, ip| "echo '#{ip} #{n_name}' | sudo tee -a /etc/hosts\n"}.join}

        # Installer Docker
        curl -fsSL get.docker.com -o get-docker.sh
        CHANNEL=stable sh get-docker.sh
        rm get-docker.sh

        # Faire en sorte que le daemon Docker soit accessible depuis l'hôte
        sudo mkdir -p /etc/systemd/system/docker.service.d
        sudo bash -c 'echo -e "[Service]\nExecStart=\nExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2375" > /etc/systemd/system/docker.service.d/options.conf'
        sudo systemctl daemon-reload
        sudo systemctl restart docker.service
      SHELL

      node.vm.provision "docker" do |d|
        d.pull_images "alpine:latest"
      end
    end
  end
end