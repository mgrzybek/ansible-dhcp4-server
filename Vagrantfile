# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = ENV['VAGANT_BOX_NAME'] || "base"
  config.vm.box_url = ENV['VAGANT_BOX_URL']

  config.vm.provider "virtualbox" do |p|
    p.cpus = ENV['VAGRANT_VM_CPUS'] || 1
    p.memory = ENV['VAGRANT_VM_MEMORY'] || 1024

    if(ENV['VAGRANT_DATA_DISK_USAGE'].downcase == "true")
      until(File.Exists?("ansible-dhcp4-server.vmdk"))
        p.customize [
          "createmedium", "disk",
          "--filename", "ansible-dhcp4-server.vmdk",
          "--format", "vmdk",
          "--size", ENV['VAGRANT_DATA_DISK_SIZE_MB'] || "10240"
        ]
      end

      p.customize [
        "storageattach", :id,
        "--storagectl", "SATA Controller",
        "--port", "1",
        "--device", "0",
        "--type", "hdd",
        "--medium", "ansible-dhcp4-server.vmdk"
      ]
    end
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "tests/main.yml"
  end
end
