# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  
  config.vm.box = "ubuntu/artful64"
  config.vm.hostname = "vivin"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder "./data", "/vagrant_data"
  config.vm.synced_folder "C://Users/yclian/Development", "/home/yclian/Development", group:'vagrant', owner:'yclian', mount_options: ["dmode=775,fmode=777"]

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant.
  
  config.vm.provider "virtualbox" do |vb|
    # If X fails to start: 1) fix permissions in $HOME, and 2) copy 
    # /etc/X11/xinit/xinitrc to ~/.xinitrc
    vb.gui = true
    vb.memory = "2048"
  end

  # $ vagrant plugin install vagrant-vbguest
  # See: https://github.com/dotless-de/vagrant-vbguest
  config.vbguest.auto_update = false
  config.vbguest.no_remote = true
  
  config.vm.provision "shell", inline: <<-SHELL

    useradd -m -s /bin/bash -U yclian -u 666 --groups admin,vagrant
    cp -pr /home/vagrant/.ssh /home/yclian/
    chown -R yclian:yclian /home/yclian
    echo "yclian:yclian" | chpasswd
    echo "%yclian ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/yclian

    apt-get update
    apt-get install -y build-essential curl net-tools screen snapd xfce4 xfce4-terminal \
      docker libvirt-bin qemu-kvm \
      git python3.6

    snap install kubectl --classic
    curl -LO minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
    curl -LO https://storage.googleapis.com/minikube/releases/latest/docker-machine-driver-kvm2 && chmod +x docker-machine-driver-kvm2 && sudo mv docker-machine-driver-kvm2 /usr/bin/
    addgroup libvirtd
    adduser yclian libvirtd
    minikube config set vm-driver kvm2

    curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64 && \
      chmod +x kops-linux-amd64 && \
      mv kops-linux-amd64 /usr/local/bin/kops

    curl https://storage.googleapis.com/kubernetes-helm/helm-v2.8.2-linux-amd64.tar.gz | tar xvz && \
      mv linux-amd64/helm /usr/local/bin/helm && \
      rm -rf linux-amd64

    pip install install awscli
    npm install -g yarn

    su -c "curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash" yclian
    
  SHELL

end
