Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/trusty64"

  config.vm.define 'loadbalancer' do |machine|
    machine.vm.network "private_network", netmask: "255.255.255.0", ip: "172.17.177.63"
    machine.vm.network "forwarded_port", guest: 80, host: 8080, auto_correct: true

    machine.vm.provision "shell", inline: "apt update && DEBIAN_FRONTEND=noninteractive apt -yq install python python-pip docker.io git && pip install docker-py"
    machine.vm.provision :ansible_local do |ansible|
      ansible.playbook       = "interview.yml"
      ansible.verbose        = true
      ansible.install        = true
      ansible.limit          = "loadbalancer" # or only "nodes" group, etc.
      ansible.inventory_path = "inventory"
    end
  end


  config.vm.define 'worker1' do |machine|
    machine.vm.network "private_network", netmask: "255.255.255.192", ip: "172.17.177.100"

    machine.vm.provision "shell", inline: "apt update && DEBIAN_FRONTEND=noninteractive apt -yq install python python-pip docker.io git && pip install docker-py"
    machine.vm.provision "shell", inline: "git clone https://github.com/ciprianc/hello_flask.git /tmp/hello_flask"
    machine.vm.provision :ansible_local do |ansible|
      ansible.playbook       = "interview.yml"
      ansible.verbose        = true
      ansible.install        = true
      ansible.limit          = "workers"
      ansible.inventory_path = "inventory"
    end
  end


  config.vm.define 'worker2' do |machine|
    machine.vm.network "private_network", netmask: "255.255.255.0", ip: "172.17.177.101"

    machine.vm.provision "shell", inline: "apt update && DEBIAN_FRONTEND=noninteractive apt -yq install python python-pip docker.io git && pip install docker-py"
    machine.vm.provision "shell", inline: "git clone https://github.com/ciprianc/hello_flask.git /tmp/hello_flask"
    machine.vm.provision :ansible_local do |ansible|
      ansible.playbook       = "interview.yml"
      ansible.verbose        = true
      ansible.install        = true
      ansible.limit          = "workers"
      ansible.inventory_path = "inventory"
    end
  end

end
