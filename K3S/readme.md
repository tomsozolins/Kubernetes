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

#### Add env vars
```
vi ~/.bash_profile
```
```
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
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

# k3sup install --cluster --user=root --ip <first-node-ip> --tls-san <VIP-address> --k3s-extra-args="--disable servicelb"
```

## Install Helm
## Install Kube-VIP with helm

#### Add rest of the master nodes (3 nodes minimum for quorum)
```
# k3sup join --ip=<node-ip-address> --server-ip=<first-node-ip> --user=root --server-user=root --server --k3s-extra-args="--disable servicelb"
```

#### Add worker nodes
```
# k3sup join --ip=<node-ip-address> --server-ip=<first-node-ip> --user=root --server-user=root
```

## Troubleshooting
```
# kubectl get all --all-namespaces
# kubectl get nodes -o wide
# kubectl get endpoints -n kube-system
# kubectl get service -n kube-system
# kubectl get endpoints kube-controller-manager --namespace=kube-system -o yaml
```

#### Garbage collection for images failed 
```
k3s crictl rmi --prune
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
