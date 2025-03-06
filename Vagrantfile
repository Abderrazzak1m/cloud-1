Vagrant.configure("2") do |config|
    #Server Node
    config.vm.define "host" do |server_conf|
        server_conf.vm.box = "ubuntu/jammy64"
        server_conf.vm.network "private_network", ip: "192.168.56.110"
        server_conf.vm.provider "virtualbox" do |vb|
            vb.name = "host"
            vb.memory = "2048"
            vb.cpus = 2
        end
        server_conf.vm.provision "shell", path: "./scripts/vm1_setup.sh"
    end
    # # remote server 1
    # config.vm.define "remote-server" do |worker_conf|
    #     worker_conf.vm.box = "ubuntu/jammy64"
    #     worker_conf.vm.network "private_network", ip: "192.168.56.111"
    #     worker_conf.vm.provider "virtualbox" do |vb|
    #         vb.name = "remote-server"
    #         vb.memory = "512"
    #         vb.cpus = 1
    #     end
    #     worker_conf.vm.provision "shell", path: "./scripts/vm2_setup.sh"
    # end
    # # remote server 2
    # config.vm.define "remote-server2" do |worker_conf|
    #     worker_conf.vm.box = "ubuntu/jammy64"
    #     worker_conf.vm.network "private_network", ip: "192.168.56.112"
    #     worker_conf.vm.provider "virtualbox" do |vb|
    #         vb.name = "remote-server2"
    #         vb.memory = "512"
    #         vb.cpus = 1
    #     end
    #     worker_conf.vm.provision "shell", path: "./scripts/vm2_setup.sh"
    # end
end

  