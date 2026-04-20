
Let's install a demo application called 

`kubectl apply -f https://raw.githubusercontent.com/odigos-io/kv-mall/refs/heads/cpp-app/prod-deploy/kv-mall-manifest/kv-mall.yaml`{{exec}}


Let's look at the front end
`sed 's/PORT/80/g' /etc/killercoda/host`{{exec}}

`kubectl port-forward --address 0.0.0.0 -n kv-mall service/frontend 8080:8080`{{exec}}

`sed 's/PORT/8080/g' /etc/killercoda/host`{{exec}}

