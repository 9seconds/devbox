# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|
  config.vm.box_check_update = false
  config.ssh.forward_agent   = true

  config.vm.define "devbox", primary: true do |devbox|
    devbox.vm.hostname = "devbox"
    devbox.vm.network "private_network", ip: "10.0.0.10"

    devbox.vm.synced_folder ".", "/vagrant",
      type:          "nfs",
      mount_options: ["rw", "vers=3", "noatime", "async"]

    devbox.vm.provider "virtualbox" do |vb, override|
      override.vm.box = "ubuntu/xenial64"

      vb.gui    = false
      vb.memory = 4096
      vb.cpus   = 3
    end

    devbox.vm.provider "libvirt" do |lv, override|
      override.vm.box = "yk0/ubuntu-xenial"

      lv.nested               = false
      lv.memory               = 4096
      lv.cpus                 = 3
      lv.cpu_mode             = "host-passthrough"
      lv.machine_virtual_size = 60
      lv.disk_bus             = "virtio"
      lv.nic_model_type       = "virtio"
    end

    devbox.vm.provision "ansible" do |ansible|
      ansible.playbook = "provision/site.yaml"
    end
end
