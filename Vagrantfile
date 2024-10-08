# -*- mode: ruby -*-
# vi: set ft=ruby :

vm_image = "ubuntu/jammy64"
vm_subnet = "192.168.56."
vm_script = <<-SCRIPT
  # Change the repository to Kakao
  sed -i 's/archive.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list
  sed -i 's/security.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list
  # Password Authentication for SSH
  sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
  sed -i 's/KbdInteractiveAuthentication no/KbdInteractiveAuthentication yes/g' /etc/ssh/sshd_config
  systemctl reload ssh
SCRIPT

vms = [
  {name: "jenkins", image: vm_image, cpus: 2, memory: 2048, ip: "#{vm_subnet}101"},
  {name: "tomcat", image: vm_image, cpus: 2, memory: 2048, ip: "#{vm_subnet}102"},
  {name: "jenkins-node", image: vm_image, cpus: 2, memory: 2048, ip: "#{vm_subnet}103"},
  {name: "docker", image: vm_image, cpus: 4, memory: 4096, ip: "#{vm_subnet}104"},
  {name: "webserver", image: vm_image, cpus: 2, memory: 2048, ip: "#{vm_subnet}105"},
  {name: "kubernetes", image: vm_image, cpus: 4, memory: 8192, ip: "#{vm_subnet}106"}
]

Vagrant.configure("2") do |config|
  vms.each do |vm|
    config.vm.define vm[:name] do |node|
      node.vm.box = vm[:image]
      node.vm.provider "virtualbox" do |vb|
        vb.linked_clone = false
        vb.name = vm[:name]
        vb.cpus = vm[:cpus]
        vb.memory = vm[:memory]
      end
      node.vm.hostname = vm[:name]
      node.vm.network "private_network", ip: vm[:ip], nic_type: "virtio"  
      node.vm.provision "shell", inline: vm_script
    end
  end
end