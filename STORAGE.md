# Kubernetes Storage Cheatsheet


Learning Checklist
- [ ] Persistent volumes
  -  [ ] Definitions and notes 
  -  [x] Examples 
- [ ] Persistent Volume Claim
  -  [ ] Definitions and notes 
  -  [ ] Examples 
- [ ] Storage Class
  -  [ ] Definitions and notes 
  -  [ ] Examples 



<b>Quick Links to examples</b>
- 
- [Persistent Volume Examples](#persistant-volume-examples)
- [Persistent volume claim examples](#persistant-volume-claim-examples)
-
-


### Persistent Volume Examples



<b>PersistentVolume Example</b>

```yaml
kind: PersistentVolume
apiVersion: v1
metadata:
  name: pv-sc-example
  labels:
    type: hostpath
spec:
  capacity:
    storage: 2Gi
  accessModes:
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Delete
  storageClassName: mypvsc
  hostPath:
    type: DirectoryOrCreate
    path: "/data/mypvsc"
```


```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: task-pv-volume
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"

```

### Persistent Volume Claim Examples
<b>PersistentVolume Claim Example</b>
```yaml
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: pvc-selector-example
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 1Gi
  selector:
    matchLabels:
      type: hostpath
```



This is a Pod example suing a PVC

```yaml

apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/var/www/html"
        name: mypd
  volumes:
    - name: mypd
      persistentVolumeClaim:
        claimName: myclaim
```

