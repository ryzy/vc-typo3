Vagrant.require_plugin "vagrant-berkshelf"
Vagrant.require_plugin "vagrant-omnibus"

Vagrant.configure("2") do |config|
  config.vm.hostname = "vc-typo3"
  
  config.vm.box = "opscode-centos-6.5"
  config.vm.box_url = "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_centos-6.5_chef-provisionerless.box"
  
  config.vm.boot_timeout = 120
  config.vm.network :private_network, ip: '192.168.66.6'
  
  config.vm.provider :virtualbox do |vb|
    vb.gui = false
    vb.customize ["modifyvm", :id, "--memory", "1024"]
    vb.customize ["modifyvm", :id, "--cpus", "8"]
  end
  
  config.vm.synced_folder "shared/", "/mnt/shared/", create:true, type: "nfs"
  config.vbguest.auto_update = true
  
  config.omnibus.chef_version = :latest
  config.berkshelf.enabled = true
  
  config.vm.provision :chef_solo do |chef|
    chef.run_list = [
      "recipe[vc-typo3::default]"
    ]
  end
end
