
Let's install a demo application called 

`kubectl apply -f kubectl apply -f https://raw.githubusercontent.com/odigos-io/simple-demo/main/kubernetes/deployment.yaml`{{exec}}
###`kubectl apply -f https://raw.githubusercontent.com/odigos-io/kv-mall/refs/heads/cpp-app/prod-deploy/kv-mall-manifest/kv-mall.yaml`{{exec}}




Install K8sGPT
`curl -LO https://github.com/k8sgpt-ai/k8sgpt/releases/download/v0.4.31/k8sgpt_amd64.deb`
`sudo dpkg -i k8sgpt_amd64.deb`

See what k8sgpt finds about our cluster
`k8sgpt analyze --explain`

Setup Gemini with a Key
`k8sgpt auth update --backend google --model gemini-2.5-flash --password "AIzaSyCZ6Cb2vPcrbz6uj1nyDzghFt3owXzl29M"`

Google is the default
`k8sgpt auth list`

Use AI to explain
`k8sgpt analyze --explain --backend google`






Let's look at the front end
`sed 's/PORT/80/g' /etc/killercoda/host`{{exec}}

`kubectl port-forward --address 0.0.0.0 -n kv-mall service/frontend 8080:8080`{{exec}}

`sed 's/PORT/8080/g' /etc/killercoda/host`{{exec}}

