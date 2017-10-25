# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "scalefactory/centos7"
  config.vm.network "public_network"
  config.vm.provider "virtualbox" do |vb|
     vb.memory = "1024"
  end
  config.vm.provision "shell", inline: <<-SHELL
  which ansible >/dev/null || yum -y install ansible
  which git >/dev/null || yum -y install git
  yum install -y python-pip
  pip install --upgrade pip
  yum install -y python-kerberos
  pip install xmltodict
  pip install pywinrm

cat >.ansible.cfg<<EOF
[defaults]
host_key_checking = False
EOF


mkdir -p group_vars
cat >group_vars/windows.yml<<EOF
ansible_user: Administrator
ansible_password: Daniel123
ansible_port: 5986
ansible_connection: winrm
ansible_winrm_server_cert_validation: ignore
EOF

cat >/etc/motd<<EOF
# update inventory file
# to test 
ansible -m win_ping all -i inventory
# to run playbook
ansible-playbook install.yml -i inventory 
EOF

git clone https://github.com/kmonticolo/ansible.windows

#ansible -m win_ping all -i inventory
SHELL
end
