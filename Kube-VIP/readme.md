## Helm install Kube-VIP
https://kube-vip.github.io/helm-charts/
#### Adding the Chart
```
helm repo add kube-vip https://kube-vip.io/helm-charts
helm repo update
helm show values kube-vip/kube-vip > kube-vip-values.yaml
```

```
config:
  vip_interface: "ethX"
  vip_address: "X.X.X.X"
```

```
helm install kube-vip kube-vip/kube-vip --values kube-vip-values.yaml --namespace kube-system
```
### Upgrading
```
helm upgrade kube-vip kube-vip/kube-vip --values kube-vip-values.yaml --namespace kube-system
```

## Regular install kube-vip
```
# export VIP=x.x.x.x
# export INTERFACE=ethx
# curl -s https://kube-vip.io/manifests/rbac.yaml > kube-vip-rbac.yaml
# curl -sL kube-vip.io/k3s | vipAddress=$VIP vipInterface=$INTERFACE sh | sudo tee kube-vip.yaml
# kubectl apply -f kube-vip-rbac.yaml
# kubectl apply -f kube-vip.yaml
```

## Modify kubeconfig to vip address
```
# vi /etc/rancher/k3s/k3s.yaml
- cluster:
     server: <VIP-address>
```
