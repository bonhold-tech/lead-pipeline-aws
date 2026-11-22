# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Libvirt-compatible equivalent of ubuntu/focal64
  config.vm.box = "generic/ubuntu2004"

  # Private network (host-only in libvirt)
  config.vm.network "private_network", ip: "192.168.56.82"

  # Public network (bridged) - libvirt usually requires specifying the host network interface
  # config.vm.network "public_network", dev: "eth0", mode: "bridge"

  # Libvirt provider configuration
  config.vm.provider :libvirt do |lv|
    lv.memory = 2048
    lv.cpus = 2
  end

  # Official Docker installation provisioning script
  config.vm.provision "shell", inline: <<-SHELL
   sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg -y

sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://docker.com | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://docker.com \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
sudo curl -L "https://github.com(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
   SHELL
end
