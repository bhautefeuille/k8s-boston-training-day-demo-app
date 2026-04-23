



To set Gemini key, execute the following command

export Gemini_API_KEY="your-api-key-here"

`curl https://raw.githubusercontent.com/kagent-dev/kagent/refs/heads/main/scripts/get-kagent | bash`{{exec}}


Install Kagent demo elements
`kagent install --profile demo`{{exec}}

Forward Kagent UI
`kubectl port-forward -n kagent svc/kagent-ui 8080:8080`{{exec}}














no needed:

installing helm.  (may be in backgroun)

```
sudo apt-get install curl gpg apt-transport-https --yes
curl -fsSL https://packages.buildkite.com/helm-linux/helm-debian/gpgkey | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/helm.gpg] https://packages.buildkite.com/helm-linux/helm-debian/any/ any main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```{{exec}}
