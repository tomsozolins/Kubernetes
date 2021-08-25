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
# vi logstash-values.yaml
```
```
service: {}
  annotations: {}
  type: LoadBalancer
  externalTrafficPolicy: Local
  loadBalancerIP: ""
  selector:
    app: logstash
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
```
logstashConfig: {}
  logstash.yml: |
    http.host: "0.0.0.0"
    path.config: /usr/share/logstash/pipeline
```
```
logstashPipeline: {}
   logstash.conf: |
    input {
      syslog {
        id => "logstash-syslog"
        host => "0.0.0.0"
        port => 5140
      }
    }
    filter {
    }
    output {
      elasticsearch {
        hosts => [ "${ES_HOSTS}" ]
        user => "${ES_USER}"
        password => "${ES_PASSWORD}"
        cacert => '/etc/logstash/certificates/ca.crt'
      }
      stdout { codec => rubydebug }
    }

```
