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

### Elasticsearch and Kibana Stack Monitoring
### https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-beat-configuration-examples.html#k8s_elasticsearch_and_kibana_stack_monitoring

#### Deploy monitored stack
```
# kubectl apply -f https://raw.githubusercontent.com/tomsozolins/Kubernetes/master/Elastic%20ECK/elastic_eck_monitored.yaml
```
#### Get elastic user credentials
```
# echo $(kubectl get secret elasticsearch-es-elastic-user -o go-template='{{.data.elastic | base64decode}}')
```


#### Deploy monitoring stack
```
# kubectl apply -f https://raw.githubusercontent.com/tomsozolins/Kubernetes/master/Elastic%20ECK/elastic_eck_monitoring.yaml
```
#### Get elastic user credentials
```
# echo $(kubectl get secret elasticsearch-monitoring-es-elastic-user -o go-template='{{.data.elastic | base64decode}}')
```
