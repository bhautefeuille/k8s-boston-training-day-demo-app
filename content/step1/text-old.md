
Let's install a demo application called 

`kubectl apply -f https://raw.githubusercontent.com/odigos-io/simple-demo/main/kubernetes/deployment.yaml`{{exec}}


What do we have?
`k get nodes`{{exec}}



Here are the pods
`k get pods -A`{{exec}}

Here are the services
`k get svc -A`{{exec}}

we forward the service to port 8080
`k port-forward --address 0.0.0.0 svc/frontend 8080:8080 &`{{exec}}


Click on the link generated to see the front end
`sed 's/PORT/8080/g' /etc/killercoda/host`{{exec}}

Install K8sGPT
`curl -LO https://github.com/k8sgpt-ai/k8sgpt/releases/download/v0.4.31/k8sgpt_amd64.deb`{{exec}}

`sudo dpkg -i k8sgpt_amd64.deb`{{exec}}

See what k8sgpt finds about our cluster
`k8sgpt analyze`{{exec}}

Setup Gemini with a Key
`k8sgpt auth add --backend google --model gemini-2.5-flash --password "AIzaSyCZ6Cb2vPcrbz6uj1nyDzghFt3owXzl29M"`{{exec}}

Google is the default
`k8sgpt auth list`{{exec}}

Use AI to explain
`k8sgpt analyze --explain --backend google`{{exec}}





Installing Grafana
```sudo apt-get install -y apt-transport-https software-properties-common wget &&
sudo mkdir -p /etc/apt/keyrings/ &&
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null &&
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list &&
sudo apt-get update && sudo apt-get install -y grafana
```{{exec}}

+++++++++++++++++
kubectl create namespace meta && kubectl create namespace prod
helm repo add grafana https://grafana.github.io/helm-charts && helm repo update
helm repo update
git clone https://github.com/grafana/alloy-scenarios.git
cd alloy-scenarios/k8s/logs
helm install --values killercoda/loki-values.yml loki grafana/loki -n meta
helm install --values grafana-values.yml grafana grafana/grafana --namespace meta

The URL is in the format http://loki-gateway.<NAMESPACE>.svc.cluster.local:80 . 

helm install --values ./k8s-monitoring-values.yml k8s grafana/k8s-monitoring -n meta

export POD_NAME=$(kubectl get pods --namespace meta -l "app.kubernetes.io/name=grafana,app.kubernetes.io/instance=grafana" -o jsonpath="{.items[0].metadata.name}") && \
kubectl --namespace meta port-forward $POD_NAME 3000 --address 0.0.0.0


generate access for port 3000






+++++++++++++++++++






`sudo systemctl start grafana-server && sudo systemctl status grafana-server`{{exec}}




`helm repo add grafana https://grafana.github.io/helm-charts && helm repo update`{{exec}}







helm install --values ./k8s-monitoring-values.yml k8s grafana/k8s-monitoring -n meta



Within the configuration file k8s-monitoring-values.yml we have defined the following:

---
cluster:
  name: meta-monitoring-tutorial

destinations:
  - name: loki
    type: loki
    url: http://loki-gateway.meta.svc.cluster.local/loki/api/v1/push


clusterEvents:
  enabled: true
  collector: alloy-logs
  namespaces:
    - meta
    - prod

nodeLogs:
  enabled: false

podLogs:
  enabled: true
  gatherMethod: kubernetesApi
  collector: alloy-logs
  labelsToKeep: ["app_kubernetes_io_name","container","instance","job","level","namespace","service_name","service_namespace","deployment_environment","deployment_environment_name"]
  structuredMetadata:
    pod: pod  # Set structured metadata "pod" from label "pod"
  namespaces:
    - meta
    - prod

# Collectors
alloy-singleton:
  enabled: false

alloy-metrics:
  enabled: false

alloy-logs:
  enabled: true
  # Required when using the Kubernetes API to pod logs
  alloy:
    mounts:
      varlog: false
    clustering:
      enabled: true

alloy-profiles:
  enabled: false

alloy-receiver:
  enabled: false









Let's look at the front end
`sed 's/PORT/80/g' /etc/killercoda/host`{{exec}}

`kubectl port-forward --address 0.0.0.0 -n kv-mall service/frontend 8080:8080`{{exec}}

`sed 's/PORT/8080/g' /etc/killercoda/host`{{exec}}





`helm install --values killercoda/loki-values.yml loki grafana/loki -n meta`{{exec}}


cat loki-values.yml 
---
loki:
  auth_enabled: false
  commonConfig:
    replication_factor: 1
  schemaConfig:
    configs:
      - from: 2024-04-01
        store: tsdb
        object_store: s3
        schema: v13
        index:
          prefix: loki_index_
          period: 24h
  ingester:
    chunk_encoding: snappy
  tracing:
    enabled: true
  pattern_ingester:
      enabled: true
  limits_config:
    allow_structured_metadata: true
    volume_enabled: true
  ruler:
    enable_api: true
  querier:
    # Default is 4, if you have enough memory and CPU you can increase, reduce if OOMing
    max_concurrent: 4

minio:
  enabled: true
      
deploymentMode: SingleBinary
singleBinary:
  replicas: 1
  resources:
    limits:
      cpu: 0.5
      memory: 1Gi
    requests:
      cpu: 0.5
      memory: 1Gi
  extraEnv:
    # Keep a little bit lower than memory limits
    - name: GOMEMLIMIT
      value: 750MiB
  tolerations:
      - key: "node-role.kubernetes.io/control-plane"
        operator: "Exists"
        effect: "NoSchedule"


chunksCache:
  # default is 500MB, with limited memory keep this smaller
  writebackSizeLimit: 10MB
  enabled: false

resultsCache:
  writebackSizeLimit: 10MB
  enabled: false

test:
  enabled: false
lokiCanary:
  enabled: false


# Zero out replica counts of other deployment modes
backend:
  replicas: 0
read:
  replicas: 0
write:
  replicas: 0

ingester:
  replicas: 0
querier:
  replicas: 0
queryFrontend:
  replicas: 0
queryScheduler:
  replicas: 0
distributor:
  replicas: 0
compactor:
  replicas: 0
indexGateway:
  replicas: 0
bloomCompactor:
  replicas: 0
bloomGateway:
  replicas: 0



