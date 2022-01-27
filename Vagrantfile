Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.network "forwarded_port", guest: 80, host: 8080

    config.vm.provision :ansible do |ansible|
      ansible.playbook = "./ansible/playbook.yml"
      ansible.raw_arguments  = "--ask-vault-pass"
    end
end
