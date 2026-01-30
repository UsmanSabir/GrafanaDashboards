Nodes



100 \* (1 - avg by(instance) (rate(node\_cpu\_seconds\_total{mode="idle"}\[$\_\_rate\_interval])))  

Alert Condition: Trigger when > 80% for 5 minutes





100 \* (1 - (node\_memory\_MemAvailable\_bytes / node\_memory\_MemTotal\_bytes))  

Alert Condition: Trigger when > 85% for 5 minutes







=====

PODs

sum by(namespace, pod) (rate(container\_cpu\_usage\_seconds\_total{container!=""}\[$\_\_rate\_interval])) /   

sum by(namespace, pod) (kube\_pod\_container\_resource\_limits{resource="cpu"}) \* 100  



Alert Condition: Trigger when > 80% of limits for 5 minutes





sum by(namespace, pod) (container\_memory\_working\_set\_bytes{container!=""}) /   

sum by(namespace, pod) (kube\_pod\_container\_resource\_limits{resource="memory"}) \* 100  

Alert Condition: Trigger when > 85% of limits for 5 minutes





========

Message



High CPU usage on node {{ $labels.instance }}  



**Node {{ $labels.instance }} has CPU usage of {{ printf "%.2f" $values.B.Value }}% (threshold: 80%)**  



**====**

**High {{ $labels.resource }} usage in pod {{ $labels.namespace }}/{{ $labels.pod }}**  



**Pod: {{ $labels.pod }}**  

**Namespace: {{ $labels.namespace }}**  

**Resource: {{ $labels.resource }}**  

**Current usage: {{ printf "%.2f" $values.B.Value }}%**  

**Limit threshold: 80%**  

**Node: {{ $labels.node }}**  







**Summary:**  

**🔴 High CPU: {{ $labels.instance }}**  



**Description:**  

**Node {{ $labels.instance }} is experiencing high CPU usage.**  



**Current CPU: {{ printf "%.2f" $values.B.Value }}%**  

**Threshold: 80%**  

**Duration: {{ $activeFor }}**  



**Action required: Investigate running processes or scale resources.**  



**=============**

**Summary:**  

**⚠️ High Memory: {{ $labels.namespace }}/{{ $labels.pod }}**  



**Description:**  

**Pod {{ $labels.pod }} in namespace {{ $labels.namespace }} is consuming high memory.**  



**Current Memory: {{ printf "%.2f" $values.B.Value }}%**  

**Memory Limit: 85%**  

**Container: {{ $labels.container }}**  

**Node: {{ $labels.node }}**  



**Alert started: {{ $activeAt }}**  

**Duration: {{ $activeFor }}**  



**Runbook: https://wiki.example.com/runbooks/high-memory**  



**===================**

**{{ define "custom.title" }}**  

  **{{ if gt (len .Alerts.Firing) 1 }}**  

    **{{ len .Alerts.Firing }} instances with high CPU**  

  **{{ else }}**  

    **High CPU on {{ (index .Alerts.Firing 0).Labels.instance }}**  

  **{{ end }}**  

**{{ end }}**  



**{{ define "custom.message" }}**  

  **{{ if gt (len .Alerts.Firing) 0 }}**  

    **🔴 \*Firing Alerts:\***  

    **{{ range .Alerts.Firing }}**  

      **• {{ .Labels.instance }} - CPU: {{ .Values.B }}%**  

    **{{ end }}**  

  **{{ end }}**  

**{{ end }}**  



**================**

**Description:**  

**📊 \*Alert Details\***  

**Instance: {{ $labels.instance }}**  

**CPU Usage: {{ printf "%.2f" $values.B.Value }}%**  

**Threshold: 80%**  



**⏰ \*Timing\***  

**Started: {{ $activeAt }}**  

**Duration: {{ $activeFor }}**  



**🔍 \*Investigation\***  

**- Check top processes: `top -n 1 -b | head -20`**  

**- View metrics: https://grafana.example.com/d/nodes?var-instance={{ $labels.instance }}**  



**📚 Runbook: https://wiki.example.com/runbooks/high-cpu**  





**==========**



**Summary:**   

**High CPU on {{ $labels.instance }}**  



**Description:**  

**Node {{ $labels.instance }} CPU usage is {{ printf "%.2f" $values.B.Value }}% (threshold: 80%)**  

**Alert active for: {{ $activeFor }}**  



