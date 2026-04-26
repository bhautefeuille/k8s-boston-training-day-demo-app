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
kind create cluster --name training --config ./misc/kind-config.yaml
```

## 4. Check the cluster

```bash
kubectl get nodes
kubectl cluster-info
```

## 5. Add k alias and 
```bash
# kubectl alias
alias k=kubectl
# enable zsh completion system (if not already enabled)
autoload -Uz compinit
compinit
# kubectl completion + make it work for `k` too
source <(kubectl completion zsh)
compdef __start_kubectl k
alias kpa='kubectl get pods -A'
```

## 6. Try it!
```bash
k get pods -A
k get svc -A
```



## 7. Let's install a demo application with a eCommerce product listing page

```bash
kubectl apply -f https://raw.githubusercontent.com/odigos-io/simple-demo/main/kubernetes/deployment.yaml
```

## 8. Check the pods
```bash
k get pods -A
```



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


ok


Installing Grafana
```sudo apt-get install -y apt-transport-https software-properties-common wget &&
sudo mkdir -p /etc/apt/keyrings/ &&
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null &&
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list &&
sudo apt-get update && sudo apt-get install -y grafana
```{{exec}}
















Let's look at the front end
`sed 's/PORT/80/g' /etc/killercoda/host`{{exec}}

`kubectl port-forward --address 0.0.0.0 -n kv-mall service/frontend 8080:8080`{{exec}}

`sed 's/PORT/8080/g' /etc/killercoda/host`{{exec}}

