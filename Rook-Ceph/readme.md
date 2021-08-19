# Rook Ceph Helm
## Deploy Rook Ceph Operator
https://rook.io/docs/rook/v1.7/helm-operator.html
```
# export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
# helm repo add rook-release https://charts.rook.io/release
# helm repo update
# helm install --namespace rook-ceph rook-ceph rook-release/rook-ceph --create-namespace
# kubectl --namespace rook-ceph get pods -l "app=rook-ceph-operator"
```

## Upgrade Rook Ceph Operator
```
# helm upgrade --namespace rook-ceph rook-ceph rook-release/rook-ceph
```

## Deploy Ceph cluster
https://github.com/rook/rook/blob/master/cluster/charts/rook-ceph/values.yaml
```
# helm repo add rook-master https://charts.rook.io/master
```

```
# vi rook-ceph-values.yaml
```

```
# Installs a debugging toolbox deployment
toolbox:
  enabled: true
  image: rook/ceph:v1.7.0
  tolerations: []
  affinity: {}

monitoring:
  # requires Prometheus to be pre-installed
  # enabling will also create RBAC rules to allow Operator to create ServiceMonitors
  enabled: true
  rulesNamespaceOverride:

# imagePullSecrets option allow to pull docker images from private docker registry. Option will be passed to all service accounts.
# imagePullSecrets:
# - name: my-registry-secret

# Writing to the hostPath is required for the Ceph mon and osd pods. Given the restricted permissions in OpenShift with SELinux,
# the pod must be running privileged in order to write to the hostPath volume, this must be set to true then.
hostpathRequiresPrivileged: true
```

```
# helm install --create-namespace --namespace rook-ceph rook-ceph-cluster \
    --set operatorNamespace=rook-ceph rook-master/rook-ceph-cluster --values rook-ceph-values.yaml

#kubectl --namespace rook-ceph get cephcluster
```

## Upgrade Ceph cluster
```
# helm upgrade --create-namespace --namespace rook-ceph rook-ceph-cluster \
    --set operatorNamespace=rook-ceph rook-master/rook-ceph-cluster -f values-override.yaml
```

## Troubleshooting

#### Fix ceph-osd volume mount fail after node reboot (rocky linux)
```
# dnf install lvm2
# vgchange -ay
```
### Ceph Toolbox
##### Wait for the toolbox pod to download its container and get to the running state
```
# kubectl -n rook-ceph rollout status deploy/rook-ceph-tools
# kubectl -n rook-ceph exec -it deploy/rook-ceph-tools -- bash
```

##### All available tools in the toolbox are ready for your troubleshooting needs
```
Example:
ceph status
ceph osd status
ceph df
rados df
```

##### When you are done with the toolbox, you can remove the deployment
```
# kubectl -n rook-ceph delete deploy/rook-ceph-tools
```

#### Ceph Dashboard
```
Load Balancer
If you have a cluster on a cloud provider that supports load balancers, you can create a service that is provisioned with a public hostname. 
The yaml is the same as dashboard-external-https.yaml except for the following property
# kubectl create -f dashboard-loadbalancer.yaml
# kubectl -n rook-ceph get service

You will see the new service rook-ceph-mgr-dashboard-loadbalancer created:

# kubectl -n rook-ceph get service

Get Dashboard login credentials (admin:<pass>):
# kubectl -n rook-ceph get secret rook-ceph-dashboard-password -o jsonpath="{['data']['password']}" | base64 --decode && echo
```
