



Create Kagent namespace

```bash
kubectl create namespace kagent
```


Create secret with Google Gemini Key

```bash
export GOOGLE_API_KEY=$GEMINI_API_KEY
echo "export GOOGLE_API_KEY=$GEMINI_API_KEY" >> ~/.bashrc
source ~/.bashrc
kubectl create secret generic kagent-gemini \
  -n kagent \
  --from-literal GOOGLE_API_KEY=${GOOGLE_API_KEY}
```


Create the Kagent resource manifest for the Google Gemini Model

```bash
cat > gemini-model.yaml <<'EOF'
apiVersion: kagent.dev/v1alpha2
kind: ModelConfig
metadata:
  name: gemini-model-config
  namespace: kagent
spec:
  apiKeySecret: kagent-gemini
  apiKeySecretKey: GOOGLE_API_KEY
  model: gemini-2.5-flash
  provider: Gemini
  gemini: {}
EOF
[ -f gemini-model.yaml ] && echo "gemini-model.yaml created"
```

Create secret with Grafana API Key

```bash
kubectl create secret generic grafana-mcp-secret \
  -n kagent \
  --from-literal GRAFANA_URL=http://grafana.meta.svc.cluster.local \
  --from-literal GRAFANA_API_KEY=XXXXXX
```


Install CRDs for Kagent 
```bash
helm install kagent-crds oci://ghcr.io/kagent-dev/kagent/helm/kagent-crds \
    --namespace kagent \
    --create-namespace
```

Create Kagent Resource object for the Google Gemini model
```bash
kubectl apply -f gemini-model.yaml
```

Install kagent

```bash
helm install kagent oci://ghcr.io/kagent-dev/kagent/helm/kagent --namespace kagent  \
  --set providers.default=gemini \
  --set providers.gemini.provider=Gemini \
  --set providers.gemini.apiKeySecretRef=kagent-gemini \
  --set providers.gemini.apiKeySecretKey=GOOGLE_API_KEY \
  --set providers.gemini.model=gemini-2.5-flash \
  --set agents.k8s-agent.enabled=true \
  --set agents.argo-rollouts-agent.enabled=false \
  --set agents.cilium-debug-agent.enabled=false \
  --set agents.cilium-manager-agent.enabled=false \
  --set agents.cilium-policy-agent.enabled=false \
  --set agents.helm-agent.enabled=false \
  --set agents.istio-agent.enabled=false \
  --set agents.kgateway-agent.enabled=true \
  --set agents.observability-agent.enabled=true \
  --set agents.promql-agent.enabled=true \
  --set tools.grafana-mcp.enabled=true \
  --set grafana-mcp.grafanaUrl=http://grafana.meta.svc.cluster.local \
  --set grafana-mcp.apiKeySecret=grafana-mcp-secret \
  --set grafana-mcp.apiKeySecretKey=GRAFANA_API_KEY \
  --set tools.querydoc.enabled=true \
  --set kagent-tools.enabled=true \
 --set querydoc.resources.requests.cpu="20m" \
 --set querydoc.resources.requests.cpu="20m" \
 --set kagent-tools.resources.requests.cpu="20m" \
 --set grafana-mcp.resources.requests.cpu="20m" \
 --set controller.resources.requests.cpu="20m" \
 --set agents.promql-agent.resources.requests.cpu="20m" \
 --set agents.observability-agent.resources.requests.cpu="20m" \
 --set agents.kgateway-agent.resources.requests.cpu="20m" \
 --set agents.k8s-agent.resources.requests.cpu="20m" \
 --set querydoc.resources.requests.memory="25Mi" \
 --set kagent-tools.resources.requests.memory="25Mi" \
 --set grafana-mcp.resources.requests.memory="25Mi" \
 --set controller.resources.requests.memory="25Mi" \
 --set agents.promql-agent.resources.requests.memory="25Mi" \
 --set agents.observability-agent.resources.requests.memory="25Mi" \
 --set agents.kgateway-agent.resources.requests.memory="25Mi" \
 --set agents.k8s-agent.resources.requests.memory="25Mi" \
 --set agents.promql-agent.resources.requests.cpu="20m" \

```



Verification

```bash
kubectl get pods -n kagent
kubectl get modelconfigs -n kagent
kubectl describe modelconfig gemini-model-config -n kagent
```


Create the Grafana Agent

```bash
cat > grafana-agent.yaml <<'EOF'
apiVersion: kagent.dev/v1alpha2
kind: Agent
metadata:
  name: grafana-agent
  namespace: kagent
spec:
  declarative:
    modelConfig: gemini-model-config
    systemMessage: |
      You are a Grafana-aware agent for cluster investigation.
      You can search dashboards, query Prometheus metrics, and read Loki logs
      from the Grafana instance. Be concise and cite the dashboards or queries you used.
    tools:
      - type: McpServer
        mcpServer:
          apiGroup: kagent.dev
          kind: RemoteMCPServer
          name: grafana-mcp
EOF
kubectl apply -f grafana-agent.yaml
```

Verify grafana-agent

```bash
kubectl get agent grafana-agent -n kagent
kubectl get remotemcpserver grafana-mcp -n kagent
```


If we have everything, we can now forward kagent UI to our port 8082

Forward Kagent UI

```bash
nohup  kubectl port-forward --address 0.0.0.0  -n kagent svc/kagent-ui 8082:8080 > /dev/null 2>&1 &
````


