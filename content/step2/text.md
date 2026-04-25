


Create Kagent namespace
`kubectl create namespace kagent`{{exec}}

Ceate secret with Google Gemini Key (NEED TO FIND A WAY T INSERT KEY)
```
kubectl create secret generic kagent-gemini \
  -n kagent \
  --from-literal GOOGLE_API_KEY="your Gemini key here"
```{{exec}}


Create the Kagent resource manifest for the Google Gemini Model
```
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
```{{exec}}





```
helm install kagent-crds oci://ghcr.io/kagent-dev/kagent/helm/kagent-crds \
    --namespace kagent \
    --create-namespace
```{{exec}}


Create Kagent Resource object for the Google Gemini model
`kubectl apply -f gemini-model.yaml`{{exec}}



```
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
```{{exec}}






#   --set database.postgres.bundled.resources='{requests":{"cpu":"20m","memory":"25Mi"}}' \




Verification
```
kubectl get pods -n kagent
kubectl get modelconfigs -n kagent
kubectl describe modelconfig gemini-model-config -n kagent
```{{exec}}


If we have everything, we can now forward kagent UI to our port 8080

Forward Kagent UI
`nohup  kubectl port-forward --address 0.0.0.0  -n kagent svc/kagent-ui 8080:8080 > /dev/null 2>&1 &`{{exec}}


Click on the link generated to see the front end
`sed 's/PORT/8080/g' /etc/killercoda/host`{{exec}}











no needed:

installing helm.  (may be in background)

```
sudo apt-get install curl gpg apt-transport-https --yes
curl -fsSL https://packages.buildkite.com/helm-linux/helm-debian/gpgkey | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/helm.gpg] https://packages.buildkite.com/helm-linux/helm-debian/any/ any main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```{{exec}}


To set Gemini key, execute the following command

export Gemini_API_KEY="your-api-key-here"

`curl https://raw.githubusercontent.com/kagent-dev/kagent/refs/heads/main/scripts/get-kagent | bash`{{exec}}


Install Kagent demo elements
`kagent install --profile demo`{{exec}}





`kubectl taint nodes controlplane node-role.kubernetes.io/control-plane:NoSchedule-`{{exec}}