

Create namespaces
```bash
kubectl create namespace meta && kubectl create namespace prod
```

Add grafana helm repo
```bash
helm repo add grafana https://grafana.github.io/helm-charts && helm repo update
```

clone grafana settings repo
```bash
git clone https://github.com/grafana/alloy-scenarios.git

```

Install Loki and Grafana

```bash
cd alloy-scenarios/k8s/logs
helm install --values killercoda/loki-values.yml loki grafana/loki -n meta
helm install --values grafana-values.yml grafana grafana/grafana --namespace meta
```

The URL is in the format http://loki-gateway.<NAMESPACE>.svc.cluster.local:80 . 

Install K8s monitoring
```bash
cd alloy-scenarios/k8s/logs
helm install --values ./k8s-monitoring-values.yml k8s grafana/k8s-monitoring -n meta
```

```bash
kubectl port-forward --namespace meta --address 0.0.0.0 svc/loki-gateway 3100:80 &
curl -H "Content-Type: application/json" -XPOST -s "http://127.0.0.1:3100/loki/api/v1/push"  --data-raw "{\"streams\": [{\"stream\": {\"job\": \"test\"}, \"values\": [[\"$(date +%s)000000000\", \"This is very important log message from an ecommerce application\"]]}]}"
```


Make Grafana UI accessible
```bash
export POD_NAME=$(kubectl get pods --namespace meta -l "app.kubernetes.io/name=grafana,app.kubernetes.io/instance=grafana" -o jsonpath="{.items[0].metadata.name}") 
kubectl --namespace meta --address 0.0.0.0 port-forward $POD_NAME 3000  &
```

Verif Loki is ready for business
```bash
kubectl run curl-test -n meta --image=curlimages/curl -it --rm -- /bin/sh -c ' curl -v http://loki-gateway.meta.svc.cluster.local/loki/api/v1/status/buildinfo'
```


