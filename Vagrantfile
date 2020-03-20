# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
Vagrant.configure("2") do |config|

  ## SSH settings
  # Do not auto-generate an insecure SSH key (keep the default).
  # Indicates local private ssh-key as first option to be used by Vagrant.
  config.ssh.insert_key = false
  config.ssh.private_key_path = ["~/.ssh/id_rsa",
                                 "~/.vagrant.d/insecure_private_key"]

  # Copy your public key as authorized key into the host,
  # allowing passwordless access for the vagrant user
  config.vm.provision "file",
                      run: 'once',
                      source: "~/.ssh/id_rsa.pub",
                      destination: "~/.ssh/authorized_keys"
  config.vm.provision "file",
                      run: 'once',
                      source: "~/.ssh/id_rsa",
                      destination: "~/.ssh/id_rsa"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  [
    'paul',
    'john',
    'george',
    'ringo',
  ].each do |short_name|
    config.vm.define short_name do |host|
      host.vm.box = "centos/7"
      host.vm.provider "hyperv" do |hv|
        hv.vmname = "#{short_name}"
        hv.memory = 768
        hv.maxmemory = 2048
        hv.cpus = 2
        hv.linked_clone = true
      end
      host.vm.hostname = "#{short_name}.dev.in.ndgit.com"
      host.vm.network "public_network", bridge: "Default Switch"
      host.vm.synced_folder ".", "/vagrant", type: "rsync",
          rsync__exclude: ".git/"
    end
  end

end
