# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

   config.vm.provision :shell, :path => 'http://bit.ly/1vgzLDC'
   config.vm.box = 'precise64'
   config.vm.box_url = 'http://files.vagrantup.com/precise64.box'

   config.vm.provider "virtualbox" do |v|
    v.memory = 2048
    v.cpus = 2
    v.customize [
      "setextradata", :id,
      "VBoxInternal/Devices/ahci/0/LUN#[0]/Config/IgnoreFlush", "1"
    ]
   end

   # thanks: http://jeremykendall.net/2013/08/09/vagrant-synced-folders-permissions/
  
  config.vm.define :dev_preadly do |config|
    # config.ssh.username = "admin"
    config.vm.network :private_network, :ip => "192.168.33.13"
    config.vm.synced_folder 'apps_preadly', '/var/apps_preadly', nfs: true
    config.vm.network :forwarded_port, guest: 27017, host: 27017 #mongodb
    config.vm.network :forwarded_port, guest: 3000, host: 3000   #rails
    config.vm.network :forwarded_port, guest: 80,   host: 80     #apache
    config.vm.provision "puppet" do |puppet|
      puppet.module_path = "modules"
      puppet.manifest_file = "dev_preadly_server.pp"
    end
  end
  
end
