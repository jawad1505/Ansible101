## chart:
https://artifacthub.io/packages/helm/novum-rgi-charts/awx

1. pull helm chart
2. un pack chart
3. update values 
cat values.yaml

```
resources:
  task:
    requests:
      cpu: 1000m
      memory: 1024Mi
service:
  type: NodePort
  port: 8052
  #annotations: {}
```

## pv:

kube apply -f awx-pv.yaml
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: k8sdata-awx
  labels:
    type: local
spec:
  volumeMode: Filesystem
  persistentVolumeReclaimPolicy: Retain
  capacity:
    storage: 20Gi
  accessModes:
    - ReadWriteOnce
  local:
    path: "/opt/awxhelm"
  nodeAffinity:
    required:
      nodeSelectorTerms:
      - matchExpressions:
        - key: kubernetes.io/hostname
          operator: In
          values:
          - kubenode01
          - kubenode02
```


## commands

```
kubectl create ns awxtower
helm repo add novum-rgi-helm https://novumrgi.github.io/helm/
kube apply -f awx-pv.yaml
helm install awxtower novum-rgi-helm/awx -f values.yaml
```

## tower creds
user: admin; password: awxPassword123

## help
https://www.linuxsysadmins.com/install-ansible-awx-on-kubernetes/
