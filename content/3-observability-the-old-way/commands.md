

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
cd alloy-scenarios/k8s/logs
```


```bash
helm install --values killercoda/loki-values.yml loki grafana/loki -n meta
helm install --values grafana-values.yml grafana grafana/grafana --namespace meta
```

The URL is in the format http://loki-gateway.<NAMESPACE>.svc.cluster.local:80 . 

install K8s monitoring
```bash
helm install --values ./k8s-monitoring-values.yml k8s grafana/k8s-monitoring -n meta
```

```bash
export POD_NAME=$(kubectl get pods --namespace meta -l "app.kubernetes.io/name=grafana,app.kubernetes.io/instance=grafana" -o jsonpath="{.items[0].metadata.name}") && \
kubectl --namespace meta port-forward $POD_NAME 3000 --address 0.0.0.0
```
