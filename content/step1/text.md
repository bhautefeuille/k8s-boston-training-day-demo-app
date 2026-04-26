## 1. Install kind

```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.31.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
kind version
```

## 2. Install kubectl

```bash
curl -LO "https://dl.k8s.io/release/stable.txt"
KUBECTL_VERSION=$(cat stable.txt)
curl -LO "https://dl.k8s.io/release/${KUBECTL_VERSION}/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/kubectl
rm stable.txt
kubectl version --client
```

## 3. Create a kind cluster with 2 nodes, a controle plane and a worker node

```bash
kind create cluster --name training --config /workspaces/k8s-boston-training-day-demo-app/misc/kind-config.yaml
```

## 4. Check the cluster

```bash
kubectl get nodes
kubectl cluster-info
```

## 5. Add k alias and 

```bash
#Make sure we are up to date, and that we have bash-completion
sudo apt-get update
sudo apt-get install -y bash-completion
```
## 6. Add kubectl completion and alias

```bash
#Add completion and alias
cat <<'EOF' >> ~/.bashrc
alias k=kubectl
source <(kubectl completion bash)
complete -o default -F __start_kubectl k
alias kpa='kubectl get pods -A'
EOF
source ~/.bashrc

```

## 7. Try it!
```bash
k get pods -A
k get svc -A
```



## 8. Let's install a demo application with a eCommerce product listing page

```bash
kubectl apply -f https://raw.githubusercontent.com/odigos-io/simple-demo/main/kubernetes/deployment.yaml
```

## 9. Check the pods
```bash
k get pods -A
```

## 10. Here are the services
```bash
k get svc -A
```

## 11. we forward the service to port 8080
```bash
k port-forward --address 0.0.0.0 svc/frontend 8080:8080 &
```


## 12. Download K8sGPT

```bash
curl -LO https://github.com/k8sgpt-ai/k8sgpt/releases/download/v0.4.31/k8sgpt_amd64.deb
```
## 13. Install K8sGPT

```bash
sudo dpkg -i k8sgpt_amd64.deb
```

## 14. See what k8sgpt finds about our cluster

```bash
k8sgpt analyze
```

## 15. Setup Gemini with a Key  (check key)
```bash
read -p "Enter Google Gemini API key: " GEMINI_API_KEY
k8sgpt auth add --backend google --model gemini-2.5-flash --password $GEMINI_API_KEY
```

## 16. Google Gemini is the default model
```bash
k8sgpt auth list
```

## 17. Use AI to explain what it found
```bash
k8sgpt analyze --explain --backend google
```

## 18. Installing Grafana (not kubes install)
```bash
sudo apt-get install -y apt-transport-https software-properties-common wget &&
sudo mkdir -p /etc/apt/keyrings/ &&
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null &&
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list &&
sudo apt-get update && sudo apt-get install -y grafana
```
















Let's look at the front end
`sed 's/PORT/80/g' /etc/killercoda/host`{{exec}}

`kubectl port-forward --address 0.0.0.0 -n kv-mall service/frontend 8080:8080`{{exec}}

`sed 's/PORT/8080/g' /etc/killercoda/host`{{exec}}

