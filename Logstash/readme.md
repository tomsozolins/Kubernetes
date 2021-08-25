#### Regular install
```
# kubectl apply -f logstash-configmap.yaml
# kubectl apply -f logstash.yaml
# kubectl apply -f logstash-service.yaml
```

#### Helm install
https://github.com/elastic/helm-charts/tree/master/logstash

```
# helm repo add elastic https://helm.elastic.co
# helm show values elastic/logstash > logstash-values.yaml
# helm install logstash elastic/logstash --values logstash-values.yaml
```
