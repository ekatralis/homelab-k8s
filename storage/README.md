# Storage in Kubernetes cluster

This document is a very short description, to be written in more detail soon enough. 

## Initial setup
The initial version of the Kubernetes cluster uses NFS as its storage backend. The NFS export for this specific version is backed by a 250GB SSD with a 150GB quota for the Kubernetes cluster.

First we must add the `nfs-csi-driver` to the cluster by running:
```bash
helm repo add csi-driver-nfs \
  https://raw.githubusercontent.com/kubernetes-csi/csi-driver-nfs/master/charts

helm repo update

helm install csi-driver-nfs \
  csi-driver-nfs/csi-driver-nfs \
  --namespace kube-system
```

Then we can introduce the storage class by running:
```bash
kubectl apply -f nfs-storageclass.yaml
kubectl get storageclass
kubectl describe storageclass nfs
```

We can then try creating a `PersistentVolumeClaim` by running:
```bash
kubectl apply -f test-pvc.yaml
kubectl get pv
kubectl get pvc
```

We can set the `nfs` storage class as default by running:
```
# Unset local-path as default
kubectl patch storageclass local-path \
  -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"false"}}}'

# Set nfs as default
kubectl patch storageclass nfs \
  -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```

