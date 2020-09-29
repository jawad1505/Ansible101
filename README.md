# What is ansible
# main components
# installing Ansible

1. What is ansible
   * agentless
   * ssh into machines 
2. main components
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
