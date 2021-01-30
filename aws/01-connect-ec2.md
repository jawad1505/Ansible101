 1) run an EC2 instance, download pem file, make sure 22 TCP is allowed in SG
 2) update hosts
 ```
[aws]
<XXXXX>.compute-1.amazonaws.com
[aws:vars]
ansible_user=ubuntu
ansible_ssh_private_key_file=/home/vagrant/awskey/ansible-aws.pem
 ```
 
 3)  ansible -m shell -a "cat /etc/os-release" aws
