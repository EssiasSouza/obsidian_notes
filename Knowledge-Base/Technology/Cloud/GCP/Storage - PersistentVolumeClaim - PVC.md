Source: #source/internet_resources  
Project: #project/devops 
Areas: #area/work
Subject: #subject/cloud
Type: #type/learning
Learning priority: #priority/P2	
Status: #status/learning
Related: [[GCP - Google Cloud Platform]]
[[Cloud Computing]]
[[Kubernetes]]

---
# PersistentVolumeClaim - PVC

Persistent Volume Claim is a request of a disk. It is not a disk itself yet.

### Persistent Volume - PV

Is the result of the PVC.

The [[Kubernetes]] asks a volume but it don't care if this is a real disk. 

The PV may be any kind of storage.

### Orphaned disks

It happens because the yaml file may not be configured correctly as **reclaimPolicy: Delete**, or there is an error when the pod is gone on a cleanup failure or cloud disk deletion failure!

---
### Investigation about Orphaned disks

1 - Discovering `storageClass`
```
kubectl get storageclass
```

2 - Checking `reclaimPolicy` is `Delete` for all

| NAME                 | PROVISIONER           | RECLAIMPOLICY | VOLUMEBINDINGMODE    | ALLOWVOLUMEEXPANSION | AGE  |
| -------------------- | --------------------- | ------------- | -------------------- | -------------------- | ---- |
| standard             | kubernetes.io/gce-pd  | **Delete**    | Immediate            | true                 | 3y   |
| standard-rwo         | pd.csi.storage.gke.io | **Delete**    | WaitForFirstConsumer | true                 | 2y   |
| premium-rwo          | pd.csi.storage.gke.io | **Retain**    | WaitForFirstConsumer | true                 | 1y   |
| ssd-fast             | pd.csi.storage.gke.io | **Delete**    | Immediate            | true                 | 320d |
| backup-retain        | pd.csi.storage.gke.io | **Retain**    | Immediate            | false                | 180d |
| nfs-shared           | example.com/nfs       | **Retain**    | Immediate            | true                 | 95d  |
| regional-balanced    | pd.csi.storage.gke.io | **Delete**    | WaitForFirstConsumer | true                 | 45d  |
| archive-cold-storage | pd.csi.storage.gke.io | **Retain**    | Immediate            | false                | 12d  |

3 - List disks from GCP/Compute/Disks