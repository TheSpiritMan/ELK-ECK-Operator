# ENV['VAGRANT_DEFAULT_PROVIDER'] = 'libvirt'
Vagrant.configure("2") do |config|
    config.vm.define "ELK" do |vm|
        vm.vm.box = "generic/ubuntu2204"
        vm.vm.hostname = "ELK"
        
        vm.vm.provider :libvirt do |vb|
            vb.cpus=6
            vb.memory=8000
            vb.default_prefix=""
        end
    end
    config.vm.define "Worker1" do |vm|
        vm.vm.box = "generic/ubuntu2204"
        vm.vm.hostname = "Worker1"
        
        vm.vm.provider :libvirt do |vb|
            vb.cpus=2
            vb.memory=2000
            vb.default_prefix=""
        end
    end
    config.vm.define "Worker2" do |vm|
        vm.vm.box = "generic/ubuntu2204"
        vm.vm.hostname = "Worker2"
        
        vm.vm.provider :libvirt do |vb|
            vb.cpus=2
            vb.memory=2000
            vb.default_prefix=""
        end
    end
    config.vm.define "Worker3" do |vm|
        vm.vm.box = "generic/ubuntu2204"
        vm.vm.hostname = "Worker3"
        
        vm.vm.provider :libvirt do |vb|
            vb.cpus=2
            vb.memory=2000
            vb.default_prefix=""
        end
    end
end
