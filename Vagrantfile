#If this box is not on your machine, vagrant up base and make that a base box. Then add that to vagrant
#$box_name = "my_hashicorp_precise32"
$box_name = "hashicorp/precise32"

def small(config)
    config.vm.provider "virtualbox" do |v|
      v.memory = 512 
      v.cpus = 1
    end
end

def medium(config)
    config.vm.provider "virtualbox" do |v|
      v.memory = 1024
      v.cpus = 2
    end
end

def large(config)
    config.vm.provider "virtualbox" do |v|
      v.memory = 4096 
      v.cpus = 4
    end
end


Vagrant.configure(2) do |config|

    config.vm.define "build_boot", autostart: false do |build_boot|
        small(config)
        build_boot.vm.box = $box_name
        build_boot.vm.hostname = "build-boot"
        build_boot.vm.network "private_network", ip: "192.168.2.2", virtualbox__intnet: "prov"
        build_boot.vm.synced_folder "netboot/", "/netboot/" 
        build_boot.vm.provision :shell, path: "provision_scripts/general.sh"
        build_boot.vm.provision :shell, path: "provision_scripts/dhcp.sh"
        build_boot.vm.provision :shell, path: "provision_scripts/tftp.sh"
        build_boot.vm.provision :shell, path: "provision_scripts/apache.sh"
    end

    config.vm.define "boot", autostart: true do |boot|
        small(config)

        boot.vm.box = "pxe_boot"
        boot.vm.hostname = "boot"
        boot.vm.network "private_network", ip: "192.168.2.2", virtualbox__intnet: "prov"
        boot.vm.synced_folder "netboot/", "/netboot/" 
    end

end



