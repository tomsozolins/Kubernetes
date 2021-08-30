## Gitea
```
# helm repo add gitea-charts https://dl.gitea.io/charts/
# helm repo update
# helm show values gitea-charts/gitea > gitea-values.yaml
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
  storageClass: longhorn-replica-3
```
```
postgresql:
  global:
    postgresql:
      postgresqlDatabase: gitea
      postgresqlUsername: gitea
      postgresqlPassword: gitea
      servicePort: 5432
  persistence:
    size: 10Gi
    storageClass: myOwnStorageClass
```

#### Deploy Gitea
```
# helm install gitea gitea-charts/gitea --values gitea-values.yaml
# kubectl --namespace default port-forward svc/gitea-http 3000:3000
```
