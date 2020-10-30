there are two ways to integrate with kubernetes
# 1. use kubectl in command

Using this we can use kubectl command, see `demos/kubectl` folder

 * create deployment.yaml with deployment/service/namespace script
 * create inventory.yaml with kubemaster ip
 * create k8s_playbook.yaml that has the ansible playbook
 * run ansible-playbook -i inventory k8s_playbook.yaml

# 2. use 'community.kubernetes.k8s' collection

demo:
demos/community.kubernetes

Official docs:
https://github.com/ansible-collections/community.kubernetes

Pre Reqs/Issues faced(solution):
1. install python3
2. install ansible using pip3
3. Add openshift library on client/host
4. create ssh key on controller and paste to authenticated_hosts on host 
