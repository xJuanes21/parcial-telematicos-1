# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Evita regenerar llaves (más rápido) y sube el timeout de arranque
  config.ssh.insert_key = false
  config.vm.boot_timeout = 600

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 1024
    vb.cpus = 2
    vb.gui = true   
  end

  # --- VM Servidor ---
  config.vm.define :servidor do |servidor|
    servidor.vm.box = "bento/ubuntu-22.04"
    servidor.vm.hostname = "servidor"

    servidor.vm.network :private_network, ip: "192.168.50.3"

    servidor.vm.synced_folder ".", "/vagrant", disabled: true

    servidor.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update -y
      sudo apt-get install -y openssh-server
      sudo systemctl enable --now ssh
    SHELL
  end

  config.vm.define :cliente do |cliente|
    cliente.vm.box = "bento/ubuntu-22.04"
    cliente.vm.hostname = "cliente"
    cliente.vm.network :private_network, ip: "192.168.50.2"

    cliente.vm.synced_folder ".", "/vagrant", disabled: true

    cliente.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update -y
      sudo apt-get install -y openssh-server
      sudo systemctl enable --now ssh
    SHELL
  end
end
