there are two ways to integrate with kubernetes
# 1. use kubectl in command

Using this we can use kubectl command, see `demos/kubectl` folder

 * create deployment.yaml with deployment/service/namespace script
 * create inventory.yaml with kubemaster ip
 * create k8s_playbook.yaml that has the ansible playbook
 * run ansible-playbook -i inventory k8s_playbook.yaml

# 2. use 'community.kubernetes.k8s' collection

### demo:
demos/community.kubernetes

### Official docs:
https://github.com/ansible-collections/community.kubernetes
https://galaxy.ansible.com/community/kubernetes
https://www.jeffgeerling.com/blog/2020/kubernetes-collection-ansible
https://docs.ansible.com/ansible/latest/collections/community/kubernetes/k8s_module.html#examples

### Pre Reqs/Issues faced(solution):
1. install python3
2. install ansible using pip3
    https://docs.ansible.com/ansible/latest/reference_appendices/python_3_support.html
    
3. Add openshift library on client/host
    https://github.com/openshift/openshift-restclient-python
    
    ```
    pip3 install openshift
    ```
    
    
4. create ssh key on controller and paste to authenticated_hosts on host 

### helm module
https://docs.ansible.com/ansible/2.10/collections/community/kubernetes/helm_module.html

### Good Examples:
https://stackoverflow.com/questions/60321485/how-to-automate-kubernetes-configuration
https://github.com/ansible/ansible/issues/50529
https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-ansible-on-ubuntu-18-04
https://github.com/ansible/ansible/issues/49463
