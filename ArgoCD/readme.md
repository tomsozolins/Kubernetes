## ArgoCD
#### Non-HA
https://argo-cd.readthedocs.io/en/stable/getting_started/
```
# kubectl create namespace argocd
# kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
# kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
# kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d ; echo
```

#### HA
https://argo-cd.readthedocs.io/en/stable/operator-manual/upgrading/overview/
https://github.com/argoproj/argo-cd/releases
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v2.1.1/manifests/ha/install.yaml
```
