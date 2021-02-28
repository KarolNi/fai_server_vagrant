# -*- mode: ruby -*-
# vi: set ft=ruby :

# status:
# - SERVER from FAI_CONFIG_SRC is still set to eth0 (vagrant local network) - needs bugfix in FAI, https://github.com/faiproject/fai/pull/106
# - add loading variables from json file
# - remove isc-dhcp-server as main use is without PXE boot (would be nice on next interface (network jack to point2poin connect system to be provisioned; with NAT and routing to internet), but fai-server is currently written for single interface operatioon)

ip = "192.168.1.17"
fai_config_src = "git+https:://github.com/faiproject/fai-config.git"  # check man fai.conf for more options

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
        sudo ainsl /etc/fai/nfsroot.conf SERVERINTERFACE=$(ip a | grep %s | awk '{print $NF}')  # ip form vagrant variable
        sudo ainsl /etc/fai/fai.conf FAI_CONFIG_SRC=%s  # value form vagrant variable
        sudo fai-setup
        sudo fai-cd -JAg /etc/fai/grub.cfg.autodiscover /vagrant/fai-autod.iso  # create fai autodicover cd with matching kernel and put it in shared directrory
    SHELL
    shell = sprintf(shell, ip, fai_config_src)
    config.vm.provision :shell, inline: shell
    config.vm.provision :shell, inline: "sudo fai-monitor -d", run: "always"  # start fai-monitor to allow autodiscover cd to work
end
