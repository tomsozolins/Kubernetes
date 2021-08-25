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

```
service: {}
  annotations: {}
  type: LoadBalancer
  loadBalancerIP: ""
  ports:
    - name: syslog
      port: 5140
      protocol: TCP
      targetPort: 5140
    - name: http
      port: 8080
      protocol: TCP
      targetPort: 8080

```
