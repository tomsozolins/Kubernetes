## Install Krew
https://krew.sigs.k8s.io/docs/user-guide/setup/install/

## Install Direct CSI
```
kubectl krew install direct-csi
kubectl direct-csi install --crd
kubectl direct-csi drives ls
# choose all the drives that direct-csi should manage and format them
kubectl direct-csi drives format --drives /dev/vdb --nodes kube-node1,kube-node2
# 'direct-csi-min-io' can now be specified as the storageclass in PodSpec.VolumeClaimTemplate
```

## Verify installation 
```
kubectl direct-csi info
```
