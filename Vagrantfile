Vagrant.configure(2) do |config|
    config.vm.define "jenkins" do |jenkins|
        jenkins.vm.provider :virtualbox do |v|
            v.memory = 6144
            v.cpus = 8
        end
        
        jenkins.vm.box = "ubuntu/jammy64"
        jenkins.vm.hostname = "jenkins"
        jenkins.vm.network "private_network", ip: "192.168.56.100", virtualbox__intnet: true
        # Jenkins Port
        jenkins.vm.network "forwarded_port", host:8080, guest:8080

        jenkins.vm.provision "ansible" do |ansible|
            ansible.verbose = "v"
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
        staging.vm.network "private_network", ip: "192.168.56.101", virtualbox__intnet: true
        # Java App Port
        staging.vm.network "forwarded_port", host:9999, guest:9999
        # Java JMX port
        #staging.vm.network "forwarded_port", host:5000, guest:5000
        # Grafana port
        staging.vm.network "forwarded_port", host:3000, guest:3000
        # Prometheus port
        #staging.vm.network "forwarded_port", host:9090, guest:9090
        
        staging.vm.provision "ansible" do |ansible|
            ansible.playbook = "playbook-docker.yaml"
        end
    end


    config.vm.define "stagingk8s" do |stagingk8s|
        stagingk8s.vm.provider :virtualbox do |v|
            v.memory = 2048
            v.cpus = 2
        end

        stagingk8s.vm.box = "ubuntu/jammy64"
        stagingk8s.vm.hostname = "stagingk8s"
        stagingk8s.vm.network "private_network", ip: "192.168.56.102", virtualbox__intnet: true
        # Java App Port
        stagingk8s.vm.network "forwarded_port", host:9998, guest:30880
        # JMX javaagent port
        #stagingk8s.vm.network "forwarded_port", host:8889, guest:8889

        stagingk8s.vm.provision "shell", inline: <<-SHELL
            sudo snap install microk8s --classic
            sudo usermod -a -G microk8s vagrant
            sudo chown -f -R vagrant ~/.kube
            SHELL
        
    end
end
