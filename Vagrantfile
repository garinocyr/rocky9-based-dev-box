require "yaml"

paths = YAML.load_file("Vagrant-synced-folder.config.yml")

Vagrant.configure("2") do |config|
  config.vm.box = "rockylinux/9"
  config.vm.box_url = "https://app.vagrantup.com/rockylinux/boxes/9/versions/4.0.0/providers/virtualbox/amd64/vagrant.box"
  config.vm.hostname = "dev-box"
  if Vagrant.has_plugin?("vagrant-vbguest") then
    config.vbguest.auto_update = false
  end
  config.vm.synced_folder paths["host_path"], paths["guest_path"]
  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--cpus", 4]
    vb.customize ["modifyvm", :id, "--memory", 6144]
    vb.customize ["modifyvm", :id, "--vram", 128]
    vb.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
  end
  config.vm.provision "ansible_local" do |ansible|
    ansible.verbose = "v"
    ansible.playbook = "vm_setup.yml"
  end
end
