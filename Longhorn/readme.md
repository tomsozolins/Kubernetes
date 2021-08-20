# Longhorn deployment

## Prepare disks
```
# parted /dev/vdX mklabel gpt unit TB mkpart primary 0% 100%
# mkfs.xfs /dev/vdX1
# mkdir -p /mnt/disk1
# mount /dev/vdX1 /mnt/disk1
# echo "/dev/vdX1 /mnt/disk1 xfs defaults 1 2" >> /etc/fstab
```

## Install Longhorn requirements
```
# dnf install -y iscsi-initiator-utils
# systemctl start iscsid
# systemctl enable iscsid
# dnf install -y nfs-utils
```

## Install Longhorn
```
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
# helm repo add longhorn https://charts.longhorn.io
# helm repo update
# helm show values longhorn/longhorn > longhorn-values.yaml

# helm install longhorn longhorn/longhorn --values longhorn-values.yaml --namespace longhorn-system --create-namespace
```

## Upgrade Longhorn
```
# helm upgrade longhorn longhorn/longhorn --values longhorn-values.yaml --namespace longhorn-system
# kubectl -n longhorn-system get pod
```

## Create custom storage class
```
# kubectl apply -f https://raw.githubusercontent.com/tomsozolins/Kubernetes/master/Longhorn/longhorn-storageclass.yaml
```

## Uninstall Longhorn
```
# helm uninstall longhorn longhorn/longhorn --namespace longhorn-system
```

## Longhorn Prometheus monitoring
https://longhorn.io/docs/1.1.2/monitoring/integrating-with-rancher-monitoring/

```
# kubectl apply -f https://raw.githubusercontent.com/tomsozolins/Kubernetes/master/Longhorn/longhorn-monitoring.yaml
```

### Import Grafana dashboard
https://grafana.com/grafana/dashboards/13032
