Issues

1: mounting make sure all path are correct.
2  make sure require packages installed and port is open (NFS)
3  make sure found file on exactly righ path.
4  to perform ec2 on k8s only 1 file is enough --https://github.com/MithunTechnologiesDevOps/Kubernates-Manifests/blob/master/pv-pvc/nfsstorageclass.yml

5 mount it using your ip 
6 make sure provide pvc without ip and path of volume is correct by this---


---
# Mongo host path rc
apiVersion: v1
kind: ReplicationController
metadata:
#  labels:
#    name: nfs-test
  namespace: cns-test
  name: nfs-test
spec:
  replicas: 1
  template:
    metadata:
      labels:
        name: nfs-test
    spec:
#      volumes:
#      - name: nfs-volume
#        persistentVolumeClaim:
#          claimName: nfs-test
      containers:
      - image: wowapptech/test:node-js
        name: nfs-test
        ports:
        - name: nfs-test
          containerPort: 3000
          hostPort: 80
        volumeMounts:
        - name: nfs-test
          mountPath: /usr/app
      volumes:
      - name: nfs-test
        persistentVolumeClaim:
          claimName: nfs-test



---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  namespace: cns-test
  name: nfs-test
#  labels:
#    type: local
spec:
#  storageClassName: manual
  resources:
   requests:
    storage: 2Gi
  accessModes:
    - ReadWriteOnce

...




---
# Mongo Node Port RC
apiVersion: v1
kind: Service
metadata:
  namespace: cns-test
  labels:
    name: nfs-test
  name: nfs-test
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: 3000
  selector:
    name: nfs-test
...



7  check path and apply all 
8  if tcp dial error face for accessing pod check nodes and restart them it will work also check kublet on worker node some time it stop working and due to this it happens.
9 if pod exec fail also check and restart all kube-system  
10  some time using ec2 may be storage exausted try delete unnessecery pvc using direct delete kubectl delete pvc <pvc-name> -n <ns name>

