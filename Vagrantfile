# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    # For a complete reference, please see the online documentation at
    # https://docs.vagrantup.com.
    config.vm.box = "bento/debian-10"
    config.vm.network "public_network", ip: "192.168.1.17"

    config.vm.provision :shell, inline: <<-SHELL
        wget -O - https://fai-project.org/download/2BF8D9FE074BCDE4.asc |  sudo apt-key add -
        echo "deb http://fai-project.org/download buster koeln" | sudo tee /etc/apt/sources.list.d/fai.list
        sudo apt update
        sudo apt upgrade -y
        sudo apt install -y  fai-quickstart
    SHELL
end
