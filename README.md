# Kubernetes metric solution



# install 

```
kubectl apply -f collect
kubectl -n metric apply -f .

// if you want install it to other namespace
// plz change [grafana-role.yaml/prometheus.yaml] namespace attr
```





### Setting Grafana

1. Enable grafana-kubenetes-app plugin

2. Add prometheus Datasource, host=  `http://metric-prometheus:9090` no auth,  Access:server

3. Add kubenetes cluster in grafana-kube-app config page

   host = `http://localhost:8001` no auth, Access:server
   PrometheusRead: the prometheus datasource just added

!!! DON'T CLICK Deploy button, JUST Click Save button









## 1. Collect Metric

### Kube-state-metric

>  Add-on agent to generate and expose cluster-level metrics. 
>
> https://github.com/kubernetes/kube-state-metrics

### Kubectl

>k8s auto install , api-server/api/v1/nodes/{node}/proxy/metrics/cadvisor
>
>Some metric about kubectl

### cAdvisor

> k8s auto install , api-server/api/v1/nodes/{node}/proxy/metrics/cadvisor
>
> container metric [cpu/mem/disk ...]

### Node-export

> Exporter for machine metrics



## 2. Save Metric

Prometheus will scrape kube-kubectl/kube-state-metric/kube-cadvisor/kube-nodeexport

https://github.com/dashbase/kubernetes-metrics-solution/blob/master/prometheus.yaml#L9-L74









## 3. Show Metric

Grafana-k8s-app

> Warning: ServiceAccount need some permission
>
> ```yaml
>   - apiGroups: [""]
> 	resources:
>       - componentstatuses
>       - endpoints
>       - namespaces
>       - nodes
>       - pods
>     verbs: ["get", "watch", "list"]
> ```









## limit collect metrics only for one namespace

### 1. Collect metric

   https://github.com/kubernetes/kube-state-metrics/pull/201

### 2. Save metric

only scrape designation namespace

```yaml
 # kube-kubectl, kube-cAdvisor, kube-nodeexport
 - source_labels: [__meta_kubernetes_namespace]
   action: keep
   regex: kube-system
```

### 3.Show metric

limit ServiceAccount: metric-grafana permission

