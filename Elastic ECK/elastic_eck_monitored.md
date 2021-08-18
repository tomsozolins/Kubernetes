#### Delete existing operator
```
# kubectl delete -f https://download.elastic.co/downloads/eck/1.7.0/crds.yaml
# kubectl delete -f https://download.elastic.co/downloads/eck/1.7.0/operator.yaml
```

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

### Reindexing from remote cluster to new cluster

#### Edit new cluster config to allow old remote cluster

```
# vi elastic_eck_monitored.yaml

apiVersion: elasticsearch.k8s.elastic.co/v1
kind: Elasticsearch
metadata:
  name: elasticsearch
spec:
  version: 7.14.0
  nodeSets:
  - name: default
    config:
      reindex.remote.whitelist: "otherhost:9200, another:9200, 127.0.10.*:9200, localhost:*"
      reindex.ssl.verification_mode: none
```

```
POST _reindex
{
  "source": {
    "remote": {
      "host": "https://elastic-host:9200",
      "username": "user",
      "password": "pass"
    },
    "index": "example-index",
    "query": {
      "match_all": {}
    }
  },
  "dest": {
    "index": "example-index"
  }
}

```

#### check status of reindexing

```
GET _tasks?actions=*reindex&detailed
```

#### cancel reindex task

```
POST _tasks/node_id:task_id/_cancel
```
