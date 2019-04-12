IMAGE_NAME = "centos/7"
N = 2

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.vm.box_download_insecure = true

  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
    v.cpus = 2
  end

  config.vm.define "k8s-master" do |master|
    master.vm.box = IMAGE_NAME
    master.vm.network "private_network", ip: "192.168.50.10"
    master.vm.hostname = "k8s-master"

    master.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/master-playbook.yml"
    end
  end

  (1..N).each do |i|
    config.vm.define "node-#{i}" do |node|
      node.vm.box = IMAGE_NAME
      node.vm.network "private_network", ip: "192.168.50.#{i + 10}"
      node.vm.hostname = "node-#{i}"

      node.vm.provision "ansible" do |ansible|
        ansible.playbook = "ansible/node-playbook.yml"
      end
    end
  end
end
