## Deploy AWX operator

#### Check release tag and apply operator manifest
https://github.com/ansible/awx-operator/releases
```
curl https://raw.githubusercontent.com/ansible/awx-operator/0.13.0/deploy/awx-operator.yaml -o awx-operator.yaml
kubectl apply -f awx-operator.yaml
```

## Deploy AWX
```
# kubectl apply -f awx.yaml
```
#### Get admin user password
```
# kubectl get secret awx-admin-password -o jsonpath="{.data.password}" | base64 --decode ; echo
```

#### Troubleshoot
```
kubectl get pods -l "app.kubernetes.io/managed-by=awx-operator"
kubectl get svc -l "app.kubernetes.io/managed-by=awx-operator"
kubectl exec -it awx-X --container awx-ee -- bash
kubectl exec -it deployment.apps/awx --container awx-ee -- bash
ansible --version

kubectl logs deployment.apps/awx --container awx-web --tail 10
kubectl logs deployment.apps/awx --container awx-task --tail 10
kubectl logs deployment.apps/awx --container awx-ee --tail 10
```

#### Manual collection install
```
kubectl exec -it deployment.apps/awx --container awx-ee -- bash
ansible-galaxy collection install community.zabbix
ansible-galaxy collection list
ls -l /home/runner/.ansible/collections/ansible_collections/
ls -l /usr/share/ansible/collections/ansible_collections/community/
```
#### Manual file based playbook configuration
##### Find awx pod id
```
kubectl get pods -l "app.kubernetes.io/managed-by=awx-operator"
kubectl exec -it awx-X --container awx-web -- bash
```
##### Create playbook directory
```
cd /var/lib/awx/projects/playbooks
mkdir playbooks
```
##### Create playbook files in this directory and use them in AWX UI
```
vi example-playbook.yaml
```
