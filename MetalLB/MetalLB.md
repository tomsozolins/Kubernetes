# MetalLB Helm installation

```
# helm repo add metallb https://metallb.github.io/metallb
# helm show values metallb/metallb > metallb-values.yaml
```


#### Edit metallb-values.yaml
```
configInline:
  address-pools:
   - name: default
     protocol: layer2
     addresses:
     - <X.X.X.X-X.X.X.X>
```

```
# export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
# helm install metallb metallb/metallb --values metallb-values.yaml --namespace metallb-system --create-namespace
```

#### Upgrading
```
# helm upgrade metallb metallb/metallb
```
