Vagrant.configure("2") do |config|
    # Master Nodes
    MasterCount = 1
    (1..MasterCount).each do |i|
        config.vm.define "master-#{i}" do |master|
            # Disclaimer: Not sure if this works, I just updated here from generic/ubuntu2004 to generic/ubuntu2204
            config.vm.box = "generic/ubuntu2204"
            master.vm.box_version = "4.3.12"
            master.vm.hostname = "master-#{i}"
            master.vm.network "private_network", ip: "192.168.56.6#{i}"

            master.vm.provider "virtualbox" do |vb|
                vb.name = "master-#{i}"
                vb.memory = 2048
                vb.cpus = 2
            end
        end
    end

    # Worker Nodes
    WorkerCount = 2
    (1..WorkerCount).each do |i|
        config.vm.define "worker-#{i}" do |worker|
            worker.vm.box = "generic/ubuntu2204"
            worker.vm.box_version = "4.3.12"
            worker.vm.hostname = "worker-#{i}"
            worker.vm.network "private_network", ip: "192.168.56.5#{i}"

            worker.vm.provider "virtualbox" do |vb|
                vb.name = "worker-#{i}"
                vb.memory = 2048
                vb.cpus = 2
            end
        end
    end

    config.vm.provision "shell" do |s|
        ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
        s.inline = <<-SHELL
            # Create xuanhaj user
            useradd -s /bin/bash -d /home/xuanhaj/ -m -G sudo xuanhaj
            echo 'xuanhaj ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers
            mkdir -p /home/xuanhaj/.ssh && chown -R xuanhaj /home/xuanhaj/.ssh
            echo #{ssh_pub_key} >> /home/xuanhaj/.ssh/authorized_keys
        SHELL
    end

end 
