# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :host1 => {
        :box_name => "centos/7",
        :ip_addr => '192.168.11.150'
  },
  :host2 => {
        :box_name => "centos/7",
        :ip_addr => '192.168.11.151'
  }
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

      config.vm.define boxname do |box|

          box.vm.box = boxconfig[:box_name]
          box.vm.host_name = boxname.to_s

          #box.vm.network "forwarded_port", guest: 3260, host: 3260+offset

          box.vm.network "private_network", ip: boxconfig[:ip_addr]

          box.vm.provider :virtualbox do |vb|
            vb.customize ["modifyvm", :id, "--memory", "200"]
            # Подключаем дополнительные диски
            #vb.customize ['createhd', '--filename', second_disk, '--format', 'VDI', '--size', 5 * 1024]
            #vb.customize ['storageattach', :id, '--storagectl', 'IDE', '--port', 0, '--device', 1, '--type', 'hdd', '--medium', second_disk]
          end
          
          box.vm.provision "shell", inline: <<-SHELL
            mkdir -p ~root/.ssh; cp ~vagrant/.ssh/auth* ~root/.ssh
            sed -i '65s/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
            systemctl restart sshd
          SHELL
=begin 
          config.vm.provision "ansible" do |ansible|
            ansible.verbose = "vv"
            ansible.playbook = "nginx.yml"
	    ansible.inventory_path = "./hosts" 
            ansible.become = "true"
          end
=end
      end
  end
end
