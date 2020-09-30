# What is ansible
# main components
# installing Ansible

1. What is ansible
   * agentless
   * ssh into machines 
2. main components
  * Playbook -> Plays -> Tasks
  * Inventory: Machines involved in task executions
  * Playbook:contains one or more play
  * module: 
    * critcal components
    * granular
  
3. installing Ansible

https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-ubuntu
```
sudo apt update
sudo apt install software-properties-common
sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```

4. Ping servers & check os version
* update hosts file
```
[linux]
172.28.128.9
172.28.128.10

[linux:vars]
ansible_user=vagrant
ansible_password=vagrant
```

Command:
```
ansible linux -m ping

ansible linux -a "cat /etc/os-release"
```

5. Check nano is installed

playbook to check if nano is installed on hosts

to run: 
```
ansible-playbook ilovenano.yaml
```

ilovenano.yaml
```
---
  - name: ilovenano    
    become: true       
    become_method: sudo
    hosts: linux       
    tasks:
      - name: ensure nano is installed
        apt:
          name: nano
          state: latest
```
to remove change to "state: absent"
