## Deploy AWX operator

#### Check release tag and apply operator manifest
https://github.com/ansible/awx-operator/releases
```
# kubectl apply -f https://raw.githubusercontent.com/ansible/awx-operator/<TAG>/deploy/awx-operator.yaml
```
#### Deploy AWX
```
# kubectl apply -f awx.yaml
```
#### Get admin user password
```
# kubectl get secret awx-admin-password -o jsonpath="{.data.password}" | base64 --decode ; echo
```

#### Troubleshoot
```
# kubectl get pods -l "app.kubernetes.io/managed-by=awx-operator"
# kubectl get svc -l "app.kubernetes.io/managed-by=awx-operator"
```
