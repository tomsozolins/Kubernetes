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
```

```
# vi logstash-values.yaml
```
```
replicas: 3
```

```
logstashConfig:
  logstash.yml: |
    http.host: "0.0.0.0"
    path.config: /usr/share/logstash/pipeline
```

```
logstashPipeline:
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

```
extraEnvs:
  - name: ES_HOSTS
    value: "https://elasticsearch-es-http:9200"
  - name: ES_USER
    value: "elastic"
  - name: ES_PASSWORD
    valueFrom:
      secretKeyRef:
        name: elasticsearch-es-elastic-user
        key: elastic
```

```
# Custom ports to add to logstash
extraPorts:
  - name: syslog
    containerPort: 5140
```

```
extraVolumes: |
  - name: cert-ca
    secret:
      secretName: elasticsearch-es-http-certs-public
```

```
extraVolumeMounts: |
  - name: cert-ca
    mountPath: "/etc/logstash/certificates"
    readOnly: true
```

```
service:
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

#### Installation
```
# helm install logstash elastic/logstash --values logstash-values.yaml
# kubectl get pods --namespace=default -l app=logstash-logstash -w
# kubectl describe svc logstash-logstash
```

#### Note about externalTrafficPolicy
```
Currently externalTrafficPolicy: Local setting does not seem to work when installing with Helm. 
https://github.com/elastic/helm-charts/pull/1327

Need to create separate LoadBalancer service. Edit logstash-service.yaml.
app=logstash-logstash

# kubectl apply -f logstash-service.yaml
```
#### Upgrade
```
# helm upgrade logstash elastic/logstash --values logstash-values.yaml
```

#### Uninstall
```
# helm uninstall logstash
```
