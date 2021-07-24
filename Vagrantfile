Vagrant.configure("2") do |config|
  config.vm.box = "4640BOX"
  config.ssh.username = "admin"
  config.ssh.private_key_path = "./acit_admin_id_rsa"
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    vb.linked_clone = true
  end

  config.vm.provision "file", source: "./ansible", destination: "/home/admin/ansible"

  config.vm.define "tododb" do |tododb|
    tododb.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.name = "TODO_DB_4640"
    end
    tododb.vm.hostname = "todoapp.bcit.local"
    tododb.vm.network "private_network", ip: "192.168.150.30"
    tododb.vm.synced_folder ".", "/vagrant", disabled: true    
    tododb.vbguest.auto_update = false
    tododb.vm.provision "ansible_local" do |ansible|
      ansible.provisioning_path = "/home/admin/ansible"
      ansible.playbook = "/home/admin/ansible/tododb.yaml"
    end
  end

  config.vm.define "todohttp" do |todohttp|
    todohttp.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.name = "TODO_HTTP_4640"
    end
    todohttp.vm.hostname = "todoapp.bcit.local"
    todohttp.vm.network "forwarded_port", guest: 80, host: 8888
    todohttp.vm.network "private_network", ip: "192.168.150.10"
    todohttp.vm.synced_folder ".", "/vagrant", disabled: true    
    todohttp.vbguest.auto_update = false
    todohttp.vm.provision "ansible_local" do |ansible|
      ansible.provisioning_path = "/home/admin/ansible"
      ansible.playbook = "/home/admin/ansible/todohttp.yaml"
    end
  end

  config.vm.define "todoapp" do |todoapp|
    todoapp.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.name = "TODO_APP_4640"
    end
    todoapp.vm.hostname = "todoapp.bcit.local"
    todoapp.vm.network "private_network", ip: "192.168.150.20"
    todoapp.vm.synced_folder ".", "/vagrant", disabled: true    
    todoapp.vbguest.auto_update = false
    todoapp.vm.provision "ansible_local" do |ansible|
      ansible.provisioning_path = "/home/admin/ansible"
      ansible.playbook = "/home/admin/ansible/todoapp.yaml"
    end
  end

end