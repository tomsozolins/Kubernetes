# Install Helm 
https://helm.sh/docs/intro/install/

```
# curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
# chmod 700 get_helm.sh
# ./get_helm.sh

# export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
# helm ls --all-namespaces
```

# Install cert-manager
https://artifacthub.io/packages/helm/cert-manager/cert-manager

##### Install the CustomResourceDefinition resources separately
```
# kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.5.1/cert-manager.crds.yaml
```
##### Create the namespace for cert-manager
```
# kubectl create namespace cert-manager
```

##### Add the Jetstack Helm repository
```
# helm repo add jetstack https://charts.jetstack.io
# helm repo update
```

```
# helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --version v1.0.4
```
```
# kubectl get pods --namespace cert-manager
```

# Install Rancher
https://rancher.com/docs/rancher/v2.5/en/installation/install-rancher-on-k8s/
```
# helm repo add rancher-stable https://releases.rancher.com/server-charts/stable
# helm repo update
```

```
# helm install rancher rancher-stable/rancher \
  --namespace cattle-system \
  --set hostname=rancher \
  --set replicas=3 \ 
  --create-namespace
```

#### Get ingress address
```
# kubectl get ingress -n cattle-system
```

#### Upgrading rancher
```
# helm upgrade rancher rancher-stable/rancher --namespace cattle-system
```

# Troubleshooting
```
# kubectl -n cattle-system rollout status deploy/rancher
# kubectl -n cattle-system get deploy rancher
# kubectl get ingress -n cattle-system
# kubectl -n cattle-system get pods -l app=rancher -o wide
# kubectl -n cattle-system logs -l app=rancher
```

#### Enable Debugging 
https://rancher.com/docs/rancher/v2.x/en/troubleshooting/logging/
```
# kubectl -n cattle-system get pods -l app=rancher --no-headers -o custom-columns=name:.metadata.name | while read rancherpod; do kubectl -n cattle-system exec $rancherpod -c rancher -- loglevel --set debug; done
```

#### Disable Debugging
```
# kubectl -n cattle-system get pods -l app=rancher --no-headers -o custom-columns=name:.metadata.name | while read rancherpod; do kubectl -n cattle-system exec $rancherpod -c rancher -- loglevel --set info; done
```
