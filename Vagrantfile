Vagrant.require_version ">= 1.8.0"

Vagrant.configure(2) do |config|

    N = 3
    (1..N).each do |machine_id|
        config.vm.define "m#{machine_id}" do |machine|
            machine.vm.hostname = "m#{machine_id}"
            machine.vm.network "private_network", ip: "192.168.56.#{20+machine_id}"
            machine.vm.box = "generic/ubuntu2204"
            # Doesnt work with virtualbox
            #machine.vm.disk :disk, size: "32GB", primary: true
            machine.ssh.insert_key = true
            
            machine.vm.provider "virtualbox" do |v|
                v.memory = 2048
                v.cpus = 2
            end

            if machine_id == N
                machine.vm.provision :ansible do |ansible|
                  # Disable default limit to connect to all the machines
                  ansible.compatibility_mode = "2.0"
                  ansible.limit = "all"
                  # Enable verbose mode
                  #ansible.verbose = "v"
                  ansible.playbook = "ansible/kube-bootstrap.yml"
                  #ansible.playbook = "ansible/facts-test.yml"
                  ansible.groups = {
                    "masters" => ["m1"],
                    "masters:vars" => {"kubernetes_role" => "control_plane"},
                    "workers" => ["m2", "m3"],
                    "workers:vars" => {"kubernetes_role" => "node"}
                  }
                end
            end
        end     
    end

    #config.vm.provider "virtualbox" do |v|
    #    v.memory = 2048
    #    v.cpus = 2
    #end
#
    #config.vm.define "master1" do |m1|
    #    m1.vm.hostname = "master1"
    #    m1.vm.network "private_network", ip: "192.168.56.21"
    #    m1.vm.box = "generic/ubuntu2004"
    #end
#
    #config.vm.define "worker1" do |w1|
    #    w1.vm.hostname = "worker1"
    #    w1.vm.network "private_network", ip: "192.168.56.22"
    #    w1.vm.box = "generic/ubuntu2004"
    #end
#
    #config.vm.define "worker2" do |w2|
    #    w2.vm.hostname = "worker2"
    #    w2.vm.network "private_network", ip: "192.168.56.23"
    #    w2.vm.box = "generic/ubuntu2004"
    #end
#
    #config.vm.define "worker3" do |w3|
    #    w3.vm.hostname = "worker3"
    #    w3.vm.network "private_network", ip: "192.168.56.24"
    #    w3.vm.box = "generic/ubuntu2004"
    #end

  
end
