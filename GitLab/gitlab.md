https://docs.gitlab.com/charts/
```
# helm repo add gitlab https://charts.gitlab.io/
# helm repo update
# helm install gitlab gitlab/gitlab \
  --set global.hosts.domain=<DOMAIN> \
  --set certmanager-issuer.email=email@example.com \
  --set global.edition=ce
```
