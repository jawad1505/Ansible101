
# Steps:
1. Create new Namespace
2. Install Postgress
3. install python
4. install ansible
5. download awx code
6. update settings for kubernetes
7. add tasks for migration wait
8. add migration wait time variable
9. update postgress settings in inventory
10. run playbook

# Pre Reqs
helm
kubectl

# Steps:
## 1. Create namespace
```
kubectl create ns awx
```
## 2.	Install python
```
Install docker modules
yum install python3 python36-docker -y
pip3 install docker-compose
```
## 3. Install postgres
```
$ helm repo add bitnami https://charts.bitnami.com/bitnami
$ helm search repo bitnami
$ helm install my-release bitnami/<chart>
```

postgres
```
helm install awx-postgres bitnami/postgresql --set volumePermissions.enabled=true
```
## 4.	Install ansible
  ```
  pip install ansible
  ansible --version
  ```
## 5. Download AWX Code
https://github.com/ansible/awx/blob/devel/INSTALL.md#kubernetes

```
git clone -b 15.0.1 https://github.com/ansible/awx.git
```
for latest releases:

https://github.com/ansible/awx/releases




## Optional.	Tune resources for low mem and cpu
vim installer/roles/kubernetes/defaults/main.yml

```
web_mem_request: 0
web_cpu_request: 0
task_mem_request: 0
task_cpu_request: 0
redis_mem_request: 0
redis_cpu_request: 0
```
## 6. Update inventory

get current context and update value for `kubernetes_context`

```
kubectl config get-contexts
// kubernetes_context=kubernetes-admin@kubernetes
```
this method is using ansible, for helm see helm folder

update inventory
```
localhost ansible_connection=local ansible_python_interpreter="/usr/bin/env python3"

[all:vars]

dockerhub_base=ansible

# Kubernetes Install
kubernetes_context=kubernetes_context=kubernetes-admin@kubernetes
kubernetes_namespace=ansible-awx
kubernetes_web_svc_type=NodePort
#Optional Kubernetes Variables
#pg_image_registry=docker.io
#pg_serviceaccount=awx
#pg_volume_capacity=5
pg_persistence_storageClass=dbstorageclass
#pg_persistence_existingclaim=postgrespvc
#pg_cpu_limit=1000
#pg_mem_limit=2

postgres_data_dir="~/.awx/pgdocker"

pg_username=awx
pg_password=awxpass
pg_database=awx
pg_port=5432

admin_user=admin
admin_password=password

create_preload_data=True
secret_key=awxsecret
```


## 7. add tasks for migration wait

Add wait time for migration
`vim installer/roles/kubernetes/defaults/main.yml`
```
postgress_migrate_wait: 90
```

add new task

`vim installer/roles/kubernetes/tasks/main.yml`

```
- name: Wait for management pod to start
  shell: |
    {{ kubectl_or_oc }} -n {{ kubernetes_namespace }} \
      get pod ansible-tower-management -o jsonpath="{.status.phase}"
  register: result
  until: result.stdout == "Running"
  retries: 60
  delay: 10

  - name: Wait for two minutes before starting with migrate database
  pause:
    seconds: "{{ postgress_migrate_wait }}"
  when: openshift_pg_activate.changed or kubernetes_pg_activate.changed
```


# 10. run playbook
```
ansible-playbook -i installer/inventory installer/install.yml
```

# 11. Access AWX
```
kubectl get svc
```
this will show the node post, get the node port and access from browser
```
http://192.168.56.2:31023
user: admin
password: password
```
## 7.	Run installer

ansible-playbook -i inventory install.yml

https://www.linuxsysadmins.com/install-ansible-awx-on-kubernetes/
important: for Kubernetes deployment
you have to comment Kubernetes-clusteer in inventories





## 3.	create a persistent Volume claim
cat postgrespvc.yaml

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: postgrespvc
  namespace: ansible-awx
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```


## 4. create a Persistent Volume
cat persistentVolume_k8sdata.yaml
```

apiVersion: v1
kind: PersistentVolume
metadata:
  name: k8sdata
  labels:
    type: local
spec:
  storageClassName: manual
  persistentVolumeReclaimPolicy: Retain
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/opt/k8sdata"

```
