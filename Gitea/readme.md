## Gitea
```
helm repo add gitea-charts https://dl.gitea.io/charts/
helm repo update
helm show values gitea-charts/gitea > gitea-values.yaml
```
#### Modify gitea-values.yaml
```
service:
  http:
    type: LoadBalancer
```
```
persistence:
  enabled: true
  # existingClaim:
  size: 20Gi
  accessModes:
    - ReadWriteOnce
  labels: {}
  annotations: {}
  storageClass: longhorn
```
```
postgresql:
  persistence:
    size: 20Gi
    storageClass: longhorn
  volumePermissions:
    enabled: true
```

#### Deploy Gitea
```
helm install gitea gitea-charts/gitea --values gitea-values.yaml
```

#### Upgrade
```
helm repo update
helm upgrade gitea gitea-charts/gitea --values gitea-values.yaml
```

##### test http
```
kubectl --namespace default port-forward svc/gitea-http 3000:3000
```
