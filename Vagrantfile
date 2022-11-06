Vagrant.configure(2) do |config|
    config.vm.define "jenkins" do |jenkins|
        jenkins.vm.provider :virtualbox do |v|
            v.memory = 6144
            v.cpus = 8
        end
        
        jenkins.vm.box = "ubuntu/jammy64"
        jenkins.vm.hostname = "jenkins"
        jenkins.vm.network "private_network", ip: "192.168.56.100"
        # Java App Port
        #jenkins.vm.network "forwarded_port", host:9999, guest:9999
        # Jenkins Port
        jenkins.vm.network "forwarded_port", host:8080, guest:8080
        #jenkins.vm.synced_folder ".", "/var/lib/jenkins"

        jenkins.vm.provision "ansible" do |ansible|
            #ansible.verbose = "v"
            ansible.playbook = "playbook-docker-jenkins.yaml"
        end
    end

    config.vm.define "staging" do |staging|
        staging.vm.provider :virtualbox do |v|
            v.memory = 2048
            v.cpus = 2
        end
        
        staging.vm.box = "ubuntu/jammy64"
        staging.vm.hostname = "staging"
        staging.vm.network "private_network", ip: "192.168.56.101"
        # Java App Port
        staging.vm.network "forwarded_port", host:9999, guest:9999
        
        staging.vm.provision "ansible" do |ansible|
            ansible.playbook = "playbook-docker.yaml"
        end
    end
end
