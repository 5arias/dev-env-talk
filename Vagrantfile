require 'yaml'
settings = YAML.load_file(File.join(__dir__, 'vagrant.yml'))

localpath = settings[:synced_folder][:localpath]
guestpath = settings[:synced_folder][:guestpath]
if settings.key?(:hardware)
  cpus = settings[:hardware][:cpus]
  memory = settings[:hardware][:memory]
else
  cpus = 2
  memory = 4096
end

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  if Vagrant.has_plugin?('vagrant-cachier')
    config.cache.scope = :box
  end

  if Vagrant.has_plugin?('vagrant-vbguest')
    config.vbguest.auto_update = true
  end

 config.vm.hostname = 'vagrant'
 config.vm.box_check_update = true
 config.vm.boot_timeout = 120

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network :private_network, ip: "192.168.33.10"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.

  config.vm.synced_folder localpath, guestpath

  config.vm.provider :virtualbox do |vb, override|
    # Use VBoxManage to customize the VM. For example to change memory:
    vb.cpus = cpus
    vb.memory = memory
    vb.gui = false
    vb.linked_clone = true
    vb.customize ["modifyvm", :id, "--ioapic", "on"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    override.vm.box = 'ubuntu/trusty64'
  end

end

# vim: ft=ruby
