# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what you're doing.
Vagrant.configure("2") do |config|
 config.vm.define "controlnode" do |controlnode|
   controlnode.vm.box = "ubuntu/focal64"
   controlnode.vm.hostname = "controlnode"
   controlnode.vm.network "private_network", ip: "192.168.10.30"
   controlnode.vm.synced_folder "C:/Ansible","/home/vagrant/ansible"
   controlnode.vm.provision "shell", inline: <<-SHELL 
      sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/#g' /etc/ssh/sshd_config 
      service ssh restart
   SHELL
end
#Every Vagrant development enviroment requires a box. You can search for boxes at https://vagrantcloud.com/search.
 config.vm.define "server" do |server|
   server.vm.box = "ubuntu/focal64"
   server.vm.hostname = "server"
   server.vm.network "private_network", ip: "192.168.10.20"
   server.vm.provision "shell", inline: <<-SHELL
     sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/#g' /etc/ssh/sshd_config
     service ssh restart
   SHELL
 end
end