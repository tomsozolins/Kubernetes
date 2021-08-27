## Deploy Gitea
```
# helm repo add gitea-charts https://dl.gitea.io/charts/
# helm show values gitea-charts/gitea > gitea-values.yaml
```
#### Modify gitea-values.yaml
```
persistence:
  enabled: true
  storageClass: myOwnStorageClass
```
```
# helm install gitea gitea-charts/gitea --values gitea-values.yaml
```
