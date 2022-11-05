Vagrant.configure(2) do |config|
    config.vm.provider :virtualbox do |v|
        v.memory = 6144
        v.cpus = 8
    end

    config.vm.box = "ubuntu/jammy64"
    config.vm.network "private_network", ip: "192.168.56.100"
    config.vm.network "forwarded_port", host:8090, guest:8090
    config.vm.network "forwarded_port", host:9999, guest:9999
    config.vm.network "forwarded_port", host:8080, guest:8080
    #config.vm.synced_folder ".", "/app"

    config.vm.provision "shell", inline: <<-SHELL
        sudo apt-get -y install ca-certificates curl gnupg lsb-release
        sudo mkdir -p /etc/apt/keyrings
        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
        echo \
            "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
            $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
        curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
            /usr/share/keyrings/jenkins-keyring.asc > /dev/null
        echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
            https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
            /etc/apt/sources.list.d/jenkins.list > /dev/null
        sudo apt-get update
        sudo apt-get -y install docker-ce docker-ce-cli containerd.io docker-compose-plugin 
        sudo apt-get -y install openjdk-11-jdk 
        sudo apt-get -y install jenkins
        sudo addgroup jenkins docker
        sudo systemctl start jenkins.service
        sudo cat /var/lib/jenkins/secrets/initialAdminPassword
        SHELL
end
