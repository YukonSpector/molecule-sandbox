DOMAIN=".molecule.local"
BOX="ubuntu/bionic64"
RAM = "512"
CPU_COUNT = "1"
CPU_CAP = "50"

servers=[
  {
    :hostname => "sandbox" + DOMAIN,
    :box => "ubuntu/bionic64",
    :ip_int_one => "1",
    :playbook => "molecule.yaml"
  }
]

Vagrant.configure(2) do |config|
    config.vm.synced_folder ".", "/vagrant", disabled: true
    servers.each do |machine|
        config.vm.define machine[:hostname] do |node|
            node.vm.box = machine[:box]
            node.vm.usable_port_range = (2200..2250)
            node.vm.hostname = machine[:hostname]
            node.vm.provider "virtualbox" do |vb|
                vb.customize ["modifyvm", :id, "--memory", RAM]
                vb.customize ["modifyvm", :id, "--cpus", CPU_COUNT]
                vb.customize ["modifyvm", :id, "--cpuexecutioncap", CPU_CAP]
            end
            if (!machine[:playbook].nil?)
                node.vm.provision "ansible" do |ansible|
                    ansible.playbook = machine[:playbook]
                end
            end
        end
    end
end
