Vagrant.configure("2") do |config|
  config.vm.box_check_update = true
  config.vm.box = "ubuntu/bionic64"

  config.vm.provider "virtualbox" do |v|
    v.name = "ubuntu"
    v.gui = true
    v.cpus = 2
    v.memory = "4096"
    v.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
  end

  config.vm.provision "shell", inline: "sudo apt-get update -y"
  config.vm.provision "shell", inline: "sudo apt-get install -y net-tools openssh-server python"

  config.vm.provision "ansible" do |ansible|
    ansible.inventory_path = "machines"
    ansible.limit = "virtualbox"
    ansible.playbook = "systems.yaml"
    ansible.extra_vars = {
      type: ENV['TYPE']
    }
  end

  if ENV['TYPE'] == 'pc'
    config.vm.provision "shell", inline: "sudo apt-get install -y gnome-session gdm3"
    config.vm.provision "shell", inline: "sudo reboot"
  end
end
