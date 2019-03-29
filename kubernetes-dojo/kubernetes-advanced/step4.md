Volume is essentially a directory accessible to all containers running in a Pod. 
Data on that filesystem will be destroyed when the container is restarted. Volumes let that a pod can write to a filesystem that exists as long as the pod exists.

A PersistentVolume (PV) is a piece of storage in the cluster that has been provisioned by an administrator. It is a resource in the cluster just like a node is a cluster resource. PVs are volume plugins like Volumes, but have a lifecycle independent of any individual pod that uses the PV. This API object captures the details of the implementation of the storage, be that NFS, iSCSI, or a cloud-provider-specific storage system.

A PersistentVolumeClaim (PVC) is a request for storage by a user. It is similar to a pod. Pods consume node resources and PVCs consume PV resources. Pods can request specific levels of resources (CPU and Memory). Claims can request specific size and access modes (e.g., can be mounted once read/write or many times read-only).

## Katas

Create an emptyDir volume and mount it to nginx pod

Update volume as a hostpath volume

Create a persistent volume

Create a persistent volume claim with 1GB

Mount PVC to your nginx pod and create an index.html under /usr/share/nginx/html folder

Delete your pod and recreate a new nginx and check if index.html is the same


