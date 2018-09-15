# -*- mode: ruby -*-
# vi: set ft=ruby :

PRIVATE_KEY_PATH = '/Users/USERNAME/.ssh/id_rsa'
PUBLIC_KEY_PATH = '/Users/USERNAME/.ssh/id_rsa.pub'

VAGRANTFILE_API_VERSION = '2'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = 'geerlingguy/centos7'
  config.vm.box_check_update = true

  # Insert Custom SSH Key
  config.ssh.insert_key = false
  config.ssh.private_key_path = [PRIVATE_KEY_PATH,
                                 '~/.vagrant.d/insecure_private_key']
  config.vm.provision 'file',
                      source: PUBLIC_KEY_PATH,
                      destination: '~/.ssh/authorized_keys'
  config.vm.provision 'shell', inline: <<-EOC
    sudo sed -i -e "\\#PasswordAuthentication yes# s#PasswordAuthentication yes#PasswordAuthentication no#g" /etc/ssh/sshd_config
    sudo systemctl restart sshd
  EOC

  # VM :: Jenkins Master
  config.vm.define 'jenkins' do |jenkins|
    jenkins_m.vm.hostname = 'Jenkins'
    jenkins_m.vm.network 'private_network', ip: '10.0.1.20'
    jenkins_m.vm.network :forwarded_port, host: 8080, guest: 8080
    jenkins_m.vm.network :forwarded_port, host: 50000, guest: 50000
    jenkins_m.vm.provider 'virtualbox' do |v|
      v.name = 'Jenkins'
      v.memory = 768
      v.cpus = 1
      v.gui = false
      v.linked_clone = true
    end
    jenkins_m.vm.provision 'ansible' do |ansible|
      ansible.compatibility_mode = '2.0'
      ansible.playbook = 'provisioning/playbook.yml'
      ansible.become = true
    end
  end
end
