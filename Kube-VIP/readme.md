## Kube-VIP
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
