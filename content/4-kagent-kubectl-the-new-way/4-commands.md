


Create Kagent namespace

```bash
kubectl create namespace kagent
```


Create secret with Google Gemini Key

```bash
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
  --set agents.kgateway-agent.enabled=false \
  --set agents.observability-agent.enabled=true \
  --set agents.promql-agent.enabled=true \
  --set tools.grafana-mcp.enabled=true \
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
 --set kagent-tools.resources.requests.cpu="20m"  \
 --set querydoc.resources.requests.memory="25Mi" \
 --set querydoc.resources.requests.memory="25Mi" \
 --set kagent-tools.resources.requests.memory="25Mi" \
 --set grafana-mcp.resources.requests.memory="25Mi" \
 --set controller.resources.requests.memory="25Mi" \
 --set agents.promql-agent.resources.requests.memory="25Mi" \
 --set agents.observability-agent.resources.requests.memory="25Mi" \
 --set agents.kgateway-agent.resources.requests.memory="25Mi" \
 --set agents.k8s-agent.resources.requests.memory="25Mi" \
  --set agents.promql-agent.resources.requests.memory="25Mi" \
 --set agents.promql-agent.resources.requests.cpu="20m" \
 --set kagent-tools.resources.requests.memory="25Mi" 
```



Verification

```bash
kubectl get pods -n kagent
kubectl get modelconfigs -n kagent
kubectl describe modelconfig gemini-model-config -n kagent
```


If we have everything, we can now forward kagent UI to our port 8082

Forward Kagent UI

```bash
nohup  kubectl port-forward --address 0.0.0.0  -n kagent svc/kagent-ui 8082:8080 > /dev/null 2>&1 &
````


