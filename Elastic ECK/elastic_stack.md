#### Deploy ECK operator

```
# helm repo add elastic https://helm.elastic.co
# helm repo update
# helm install elastic-operator elastic/eck-operator -n elastic-system --create-namespace
```
Monitor the operator logs
```
# kubectl logs -n elastic-system sts/elastic-operator
```
```
// # kubectl -n elastic-system logs -f statefulset.apps/elastic-operator
```

#### Upgrade ECK operator
```
# helm upgrade elastic-operator elastic/eck-operator -n elastic-system
```

Create Kibana saved objects encrypted key:
```
# kubectl create secret generic kibana-saved-objects-encrypted-key --from-literal=xpack.encryptedSavedObjects.encryptionKey="$(cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w 32 | head -n 1)"

# kubectl apply -f https://raw.githubusercontent.com/tomsozolins/Kubernetes/master/Elastic%20ECK/elastic_stack.yaml
```
------------------
Useful commands
```
# kubectl get elasticsearch
# kubectl get service elasticsearch-es-http
# kubectl get elasticsearch -n logging
```
#### Get elastic user credentials
```
# echo $(kubectl get secret elasticsearch-es-elastic-user -o go-template='{{.data.elastic | base64decode}}')
```
#### Get external IP
```
# kubectl get service elasticsearch-es-http
```
To test it you must first download the automatically generated SSL certificate. You can do it by running
```
# kubectl get secret elasticsearch-es-http-certs-public -o 'go-template={{index .data "ca.crt"}}' | base64 --decode >> ca.crt
```
Then use to perform a GET request at https://<external-ip>:9200


#### Kibana
#### Get Kibana encrypted key
  
```
# kubectl get secret kibana-saved-objects-encrypted-key -o yaml
# echo -n "<secret-value>" | base64 --decode
# kubectl delete secret kibana-saved-objects-encrypted-key
```
  
#### Get external IP
```
# kubectl get service kibana-kb-http
```
