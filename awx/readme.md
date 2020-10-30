https://github.com/ansible/awx/blob/devel/INSTALL.md

# Pre Reqs

## 1.	Install python
## 2.	Install ansible
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

## 4. Clone awx repo
https://github.com/ansible/awx/blob/devel/INSTALL.md#kubernetes

## 5.	Tune resources for low mem and cpu
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
```
this method is using ansible, for helm see helm folder

update inventory
```
localhost ansible_connection=local ansible_python_interpreter="/usr/bin/env python3"

[all:vars]

dockerhub_base=ansible

# Kubernetes Install
kubernetes_context=awx-contx 
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
## 7.	Run installer

ansible-playbook -i inventory install.yml

https://www.linuxsysadmins.com/install-ansible-awx-on-kubernetes/
important: for Kubernetes deployment
you have to comment Kubernetes-clusteer in inventories


Install docker modules
yum install python3 python36-docker -y
pip3 install docker-compose
