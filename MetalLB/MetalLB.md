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
# helm install metallb metallb/metallb -f metallb-values.yaml
```
