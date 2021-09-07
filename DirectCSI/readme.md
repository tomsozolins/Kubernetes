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
export PATH="${KREW_ROOT:-$HOME/.krew}/bin:$PATH"
kubectl direct-csi info
```

## Upgrading Direct-CSI
```
kubectl direct-csi uninstall
kubectl krew upgrade direct-csi
kubectl direct-csi install
kubectl direct-csi info
```
