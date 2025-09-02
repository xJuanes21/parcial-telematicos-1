# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Evita regenerar llaves (más rápido) y sube el timeout de arranque
  config.ssh.insert_key = false
  config.vm.boot_timeout = 600

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 1024
    vb.cpus = 2
    vb.gui = false   # arranca sin GUI, evita cuelgues
  end

  # --- VM Maestro DNS ---
  config.vm.define :maestro do |maestro|
    maestro.vm.box = "ubuntu/jammy64"
    maestro.vm.hostname = "maestro.empresa.local"
    maestro.vm.network :private_network, ip: "192.168.50.3"

    maestro.vm.synced_folder ".", "/vagrant", disabled: true

    maestro.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update -y
      sudo apt-get install -y openssh-server bind9 bind9utils bind9-doc
      sudo systemctl enable --now ssh
      sudo systemctl enable --now bind9
    SHELL
  end

  # --- VM Esclavo DNS ---
  config.vm.define :esclavo do |esclavo|
    esclavo.vm.box = "ubuntu/jammy64"
    esclavo.vm.hostname = "esclavo.empresa.local"
    esclavo.vm.network :private_network, ip: "192.168.50.4"

    esclavo.vm.synced_folder ".", "/vagrant", disabled: true

    esclavo.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update -y
      sudo apt-get install -y openssh-server bind9 bind9utils bind9-doc
      sudo systemctl enable --now ssh
      sudo systemctl enable --now bind9
    SHELL
  end

  # --- VM Cliente ---
  config.vm.define :cliente do |cliente|
    cliente.vm.box = "ubuntu/jammy64"
    cliente.vm.hostname = "cliente.empresa.local"
    cliente.vm.network :private_network, ip: "192.168.50.5"

    cliente.vm.synced_folder ".", "/vagrant", disabled: true

    cliente.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update -y
      sudo apt-get install -y openssh-server dnsutils
      sudo systemctl enable --now ssh
    SHELL
  end
end
