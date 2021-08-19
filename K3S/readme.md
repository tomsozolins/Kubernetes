# K3S cluster deployment
## Prepare OS
```
# vi /etc/hostname
# vi /etc/hosts
# vi /etc/chrony.conf
pool <ntp-server-address> iburst prefer
# systemctl restart chronyd
```

#### Add private ssh key to ~/.ssh/id_rsa only on first node
```
# vi ~/.ssh/id_rsa
# chmod 400 ~/.ssh/id_rsa
```

#### Unable to ssh to remote server fix
```
# vi ~/.ssh/authorized_keys
comment this line, if exists ->
no-port-forwarding,no-agent-forwarding,no-X11-forwarding,command="echo 'Please login as the user \"ubuntu\" rather than the user \"root\".';echo;sleep 10"
```

## K3S installation with K3SUP
https://github.com/alexellis/k3sup

#### On first master node - install K3SUP
```
# curl -sLS https://get.k3sup.dev | sh

# k3sup install --cluster --user=root --ip <node-ip-address> --tls-san <VIP-address> --k3s-extra-args="--disable servicelb"
```

## Kube-VIP installation for K3S distribution
```
# export VIP=x.x.x.x
# export INTERFACE=ethx
# curl -s https://kube-vip.io/manifests/rbac.yaml > /var/lib/rancher/k3s/server/manifests/kube-vip-rbac.yaml
# curl -sL kube-vip.io/k3s | vipAddress=$VIP vipInterface=$INTERFACE sh | sudo tee /var/lib/rancher/k3s/server/manifests/kube-vip.yaml
# vi /etc/rancher/k3s/k3s.yaml
- cluster:
     server: <VIP-address>
```

#### Add master nodes (3 min for quorum)
```
# k3sup join --ip=<node-ip-address> --server-ip=<VIP-address> --user=root --server-user=root --server --k3s-extra-args="--disable servicelb"
```

#### Add worker nodes
```
# k3sup join --ip=<node-ip-address> --server-ip=<VIP-address> --user=root --server-user=root
```

## Troubleshooting
```
# kubectl get all --all-namespaces
# kubectl get nodes -o wide
# kubectl get endpoints -n kube-system
# kubectl get service -n kube-system
# kubectl get endpoints kube-controller-manager --namespace=kube-system -o yaml
```

## Uninstall K3S

#### master node
```
# /usr/local/bin/k3s-uninstall.sh
```

#### worker node
```
# /usr/local/bin/k3s-agent-uninstall.sh
```

## Deploy K3S system upgrade controller
```
# kubectl apply -f https://raw.githubusercontent.com/rancher/system-upgrade-controller/master/manifests/system-upgrade-controller.yaml
# kubectl apply -f https://raw.githubusercontent.com/rancher/system-upgrade-controller/master/examples/k3s-upgrade.yaml
```
