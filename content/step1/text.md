
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

