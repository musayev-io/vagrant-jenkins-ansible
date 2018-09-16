# Vagrant :: Jenkins
Run the latest Jenkins version on CentOS 7 with Docker-CE installed.

## Custom SSH key
With this script, Vagrant will inject your own custom SSH key instead of using the one Vagrant automatically generates for you.

If you want to use this feature, update the variables `PRIVATE_KEY_PATH` and `PUBLIC_KEY_PATH` inside the Vagrantfile to the SSH key you want to use.

If you don't want to inject your custom SSH keys and prefer to use the ones Vagrant auto-generates, comment out everything under `# Insert Custom SSH Key` in the Vagrantfile.

# Installation
Build the Vagrant box
```
vagrant up
```
Access the Jenkins server on your host
```
http://localhost:8080
```

# Initial Jenkins Setup
When you browse to `http://localhost:8080`, you will be prompted for an initial administrator password. To find this, SSH in into your Vagrant box and issue `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
