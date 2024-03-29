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

* setup EC2 as aws-worker
import the stack in cloud-fromation-template  
get the public ip and key  
paste the public key in same dir
```
cat hosts.ini

[workers]
192.168.10.10
[workers:vars]
ansible_user=vagrant

[aws]
18.221.207.114

[aws:vars]
ansible_user=ec2-user
ansible_ssh_private_key_file=awsdev-keypair.pem

```

* install python/pip/ansible on Controller
```
# python
sudo apt-get update
sudo apt-get install python
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
you can manually copy id_rsa.pub from master to child authorized_hosts

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

* Use adhoc ping to test
ansible -i hosts.ini example -m ping 
```
192.168.10.10 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

* Use adhoc to check free memory
```
ansible   -i hosts.ini workers -a "free -m"
```

* Use adhoc to check free memory and ping on aws
```
ansible -i hosts.ini aws -a "free -m"

ansible -i hosts.ini aws -m ping 
```
