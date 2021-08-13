### Elasticsearch and Kibana Stack Monitoring
### https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-beat-configuration-examples.html#k8s_elasticsearch_and_kibana_stack_monitoring

#### Deploy monitored stack
```
# kubectl apply -f https://raw.githubusercontent.com/tomsozolins/Kubernetes/c31d14df7c741bcc706bd570d4fab5fa1a13c013/Elastic%20ECK/elastic_eck_monitored.yaml
```
### Get elastic user credentials
```
# echo $(kubectl get secret elasticsearch-es-elastic-user -o go-template='{{.data.elastic | base64decode}}')
```


#### Deploy monitoring stack
```
# kubectl apply -f https://raw.githubusercontent.com/tomsozolins/Kubernetes/master/Elastic%20ECK/elastic_eck_monitoring.yaml
```
### Get elastic user credentials
```
# echo $(kubectl get secret elasticsearch-monitoring-es-elastic-user -o go-template='{{.data.elastic | base64decode}}')
```
