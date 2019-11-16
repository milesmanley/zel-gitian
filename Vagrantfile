# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure(2) do |config|

  config.vagrant.plugins = {
    "vagrant-disksize" => {"version" => "0.1.3"},
    "vagrant-scp" => {"version" => "0.5.7"}
  }

  config.ssh.forward_agent = true
  config.disksize.size = '24GB'
  config.vm.define 'zelcash-build', autostart: false do |gitian|
    gitian.vm.box = "debian/stretch64"
    gitian.vm.box_version = "9.9.0"
    gitian.vm.network "forwarded_port", guest: 22, host: 2200, auto_correct: true
    gitian.vm.provision "ansible" do |ansible|
      ansible.playbook = "gitian.yml"
      ansible.verbose = 'vvv'
      ansible.raw_arguments = Shellwords.shellsplit(ENV['ANSIBLE_ARGS']) if ENV['ANSIBLE_ARGS']
    end
    gitian.vm.provider "virtualbox" do |v|
      v.name = "zelcash-build"
      v.memory = 4096
      v.cpus = 2
    end
#    gitian.vm.synced_folder "~/.gnupg", "/home/vagrant/.gnupg", type: "sshfs"
#    gitian.vm.synced_folder "./gitian.sigs", "/home/vagrant/gitian.sigs", create: true
#    gitian.vm.synced_folder "./zelcash-binaries", "/home/vagrant/zelcash-binaries", create: true
    gitian.vm.post_up_message = "Zelcash deterministic build environment started."
  end

end
