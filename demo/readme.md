# on virtual box

* Setup a VM for ansible-controller
```
ip = 192.168.56.21
hostname = ansible
```
* setup a VM for worker
```
ip = 192.168.10.10
hostname = ubuntu20
```
* install python/pip/ansible on Controller
```
# python
sudo apt-get update
sudo apt-get install python3.8.6 # change version as latest
python3 --version

# pip
sudo apt install python3-pip

# ansible
sudo pip3 install ansible
ansible --version

ansible [core 2.11.1]
  config file = None
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/local/lib/python3.8/dist-packages/ansible
  ansible collection location = /home/vagrant/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/local/bin/ansible
  python version = 3.8.6 (default, May 27 2021, 13:28:02) [GCC 10.2.0]
  jinja version = 2.11.2
  libyaml = True
```

* generate ssh key on ansible
key pair will be generated in ~/.ssh as id_rsa and id_rsa.pub
```
ssh-keygen -t rsa
```

* copy public key to worker
copy the id_rsa.pub and move that to worker ~/.ssh/authorized_keys
```
ssh-copy-id -i ~/.ssh/<public_key_file> <user>@<remote machine>

#verify on worker
cat ~/.ssh/authorized_keys
```

* create hosts file
```
mkdir first-project
cd first-project
touch hosts.ini
```

add worker in hosts.ini
```
[workers]
192.168.10.10
```

* use adhoc ping to test
ansible -i hosts.ini example -m ping -u vagrant