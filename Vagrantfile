# -*- mode: ruby -*-
# vi: set ft=ruby :

# status:
# - SERVER from FAI_CONFIG_SRC is still set to eth0 (vagrant local network) - probably edit /etc/fai/fai.conf
# - modules mismatch - provide correct autodiscover cd - probably by vagrant shared dir

ip = "192.168.1.17"


Vagrant.configure("2") do |config|
    # For a complete reference, please see the online documentation at
    # https://docs.vagrantup.com.
    config.vm.box = "bento/debian-10"
    config.vm.network "public_network", ip: ip

    shell = <<-SHELL
        wget -O - https://fai-project.org/download/2BF8D9FE074BCDE4.asc |  sudo apt-key add -
        echo "deb http://fai-project.org/download buster koeln" | sudo tee /etc/apt/sources.list.d/fai.list
        sudo apt update
        sudo apt upgrade -y
        sudo apt install -y fai-quickstart  # it is meta package which installs fai-server, fai-doc, isc-dhcp-server, nfs-kernel-server, tftpd-hpa | atftpd, reprepro, xorriso, squashfs-tools, binutils, openbsd-inetd | inet-superserver
        sudo ainsl /etc/fai/nfsroot.conf SERVERINTERFACE=$(ip a | grep %s | awk '{print $NF}')
        sudo fai-setup
    SHELL
    shell = sprintf(shell, ip)
    config.vm.provision :shell, inline: shell
end
