

Download K8sGPT

```bash
curl -LO https://github.com/k8sgpt-ai/k8sgpt/releases/download/v0.4.31/k8sgpt_amd64.deb
```
Install K8sGPT

```bash
sudo dpkg -i k8sgpt_amd64.deb
```

See what k8sgpt finds about our cluster

```bash
k8sgpt analyze
```

Setup Gemini with a Key
```bash
k8sgpt auth add --backend google --model gemini-2.5-flash --password $GEMINI_API_KEY
```

Google Gemini is the default model
```bash
k8sgpt auth list
```

Use AI to explain what it found
```bash
k8sgpt analyze --explain --backend google
```
