# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.define "web01" do |web|
        web.vm.network "private_network", ip: "192.168.200.3"
        web.vm.provision "shell", path: "web.sh"
        web.vm.synced_folder "./web01", "/var/www/html"
  	web.vm.provider :virtualbox do |vb|
		vb.customize [ 'modifyvm', :id, '--memory', '386' ]
		vb.customize [ 'modifyvm', :id, '--cpus', '1' ]
		vb.customize [ 'modifyvm', :id, '--name', 'web01' ]
  	end
  end
  config.vm.define "web02" do |web|
        web.vm.network "private_network", ip: "192.168.200.4"
        web.vm.provision "shell", path: "web.sh"
        web.vm.synced_folder "./web02", "/var/www/html"
  	web.vm.provider :virtualbox do |vb|
		vb.customize [ 'modifyvm', :id, '--memory', '386' ]
		vb.customize [ 'modifyvm', :id, '--cpus', '1' ]
		vb.customize [ 'modifyvm', :id, '--name', 'web02' ]
  	end
  end
  config.vm.define "haproxy" do |hap|
        hap.vm.network "private_network", ip: "192.168.200.5"
        hap.vm.provision "shell", path: "haproxy.sh"
        hap.vm.network "forwarded_port", guest: 80, host: 1234
  	hap.vm.provider :virtualbox do |vb|
		vb.customize [ 'modifyvm', :id, '--memory', '386' ]
		vb.customize [ 'modifyvm', :id, '--cpus', '1' ]
		vb.customize [ 'modifyvm', :id, '--name', 'hap' ]
  	end
  end
end
