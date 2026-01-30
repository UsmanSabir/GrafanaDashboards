Nodes

100 * (1 - avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[$__rate_interval])))  
Alert Condition: Trigger when > 80% for 5 minutes


100 * (1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes))  
Alert Condition: Trigger when > 85% for 5 minutes



=====
PODs
sum by(namespace, pod) (rate(container_cpu_usage_seconds_total{container!=""}[$__rate_interval])) /   
sum by(namespace, pod) (kube_pod_container_resource_limits{resource="cpu"}) * 100  

Alert Condition: Trigger when > 80% of limits for 5 minutes


sum by(namespace, pod) (container_memory_working_set_bytes{container!=""}) /   
sum by(namespace, pod) (kube_pod_container_resource_limits{resource="memory"}) * 100  
Alert Condition: Trigger when > 85% of limits for 5 minutes


===========
Alerts
GatewayError
increase(fastapi_responses_total{client="BMW", environment="prod",status_code=~"502|504"}[1m]) > 0

1 min evaluation interval

No Data: Normal

HTTP {{ $labels.status_code }} error on {{ $labels.app_name}}

Gateway error ({{ $labels.status_code }}) detected on {{ $labels.path }} in {{ $labels.environment }} environment

https://54.156.154.167/grafana/


=============
App Alerts

group(present_over_time(fastapi_app_info{}[24h])) by (app_name,client)
unless
group(present_over_time(fastapi_app_info{}[5m])) by (app_name,client)


Service {{ $labels.app_name }} ({{ $labels.client}}) is DOWN

The Unity service '{{ $labels.app_name }}' on {{ $labels.client}} is not reporting metrics (fastapi_app_info=1).

===================

absent(increase(http_requests_total{handler="/documents/health-point", status!="2xx"}[3m]))
