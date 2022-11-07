# Welcome to the CI/CD Demo

Woot;

## Prerequisites

The CI/CD demo is using Virtualbox and Vagrant to setup a virtual machine with docker and Jenkins. The reason is not to pollute our Laptop/Desktop.

Tested with Ubuntu 22.04 (Laptop), Vagrant 2.3.2

### Install ansible-core
```
$ python3 -m venv venv-ansible-core
$ source ~/venv-ansible-core/bin/activate
$ python3 -m pip install ansible-core
```


### Install Vagrant
You can find the Vagrant installation guide [here](https://developer.hashicorp.com/vagrant/downloads?host=www.vagrantup.com)

In this repository you can find the `Vagrantfile`. To have enough memory for the compilation of the bakery-app I recommend at least 6GB of memory. 

Just run `vagrant up` - this command will setup a Ubuntu 22.04 box with docker and Jenkins.

### Jenkins
For the initial Jenkins setup, you have to copy the `initialAdminPassword` into the Jenkins Webui http://localhost:8080.

You can get the initialAdminPassword with the following command:

`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

After that you have to create your own Jenkins user - the webui will guide you.

As long as you are not destroying your virtual machine, your state will be saved into the virtual machine.

## Steps (Note for me:)
### Build
```
$ docker build --rm -t bakery-app .
```

## Junit Tests
```
mvn --batch-mode -Dmaven.test.failure.ignore=true test
```


```
$ wget https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.9.1/junit-platform-console-standalone-1.9.1.jar

$ java -jar junit-platform-console-standalone-1.9.1.jar --class-path target --scan-class-path
```

## Run the pipeline

I'v chosen the declerative pipeline, because for this simple use case it's easier to read and has an integration to the Blue Ocean interface.

You can find the declerative pipeline source code in the `Jenkinsfile`

The Junit test results are also saved


### Monitoring

1. edit src/main/resources/application.properties
add: spring.jmx.enabled=true

edit: 

$ jconsole service:jmx:rmi:///jndi/rmi://localhost:5000/jmxrmi