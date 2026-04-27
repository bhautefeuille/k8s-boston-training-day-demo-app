GEMINI API KEY

```bash
export KEY="[Enter your Gemini API key]"
echo "export GEMINI_API_KEY=$KEY" >> ~/.bashrc
curl -s "https://generativelanguage.googleapis.com/v1beta/models?key=${GEMINI_API_KEY}" | grep -q '"models"' && echo "Gemini API key is valid" || echo "Gemini API key is invalid or has no access"
```