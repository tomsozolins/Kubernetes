# Fluent Bit deployment
```
# helm repo add fluent https://fluent.github.io/helm-charts
# helm search repo fluent
# helm show values fluent/fluent-bit > fluentbit-values.yaml
```

```
## https://docs.fluentbit.io/manual/administration/configuring-fluent-bit/configuration-file
# vi fluentbit-values.yaml
```

```
env:
- name: FLUENT_ELASTICSEARCH_HOST
  value: "elasticsearch-es-http"
- name: FLUENT_ELASTICSEARCH_PORT
  value: "9200"
- name: FLUENT_ELASTICSEARCH_USER
  value: "elastic"
- name: FLUENT_ELASTICSEARCH_PASSWORD
  valueFrom:
    secretKeyRef:
      name: elasticsearch-es-elastic-user
      key: elastic

config:
  ## https://docs.fluentbit.io/manual/pipeline/outputs
  outputs: |
    [OUTPUT]
        Name            es
        Match           *
        Host            ${FLUENT_ELASTICSEARCH_HOST}
        Port            ${FLUENT_ELASTICSEARCH_PORT}
        HTTP_User       ${FLUENT_ELASTICSEARCH_USER}
        HTTP_Passwd     ${FLUENT_ELASTICSEARCH_PASSWORD}
        Logstash_Format On
        Replace_Dots    On
        Retry_Limit     False
        TLS             On
        TLS.verify      Off

## https://docs.fluentbit.io/manual/pipeline/inputs
inputs: |
  [INPUT]
     Name     syslog
     Parser   syslog-rfc5424
     Listen   0.0.0.0
     Port     5140
     Mode     tcp

```



#### Installation
```
# helm install fluent-bit fluent/fluent-bit --values fluentbit-values.yaml
```

#### Upgrading
```
# helm upgrade fluent-bit fluent/fluent-bit --values fluentbit-values.yaml
```
