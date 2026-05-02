GEMINI API KEY

Enter this line in the terminal below. Replace XXXXXXXXXXXXXXXXXXXX by your GEMINI API KEY:

```bash copy
echo "export GEMINI_API_KEY=XXXXXXXXXXXXXXXXXXXX" >> ~/.bashrc
```

Verify your Gemini API Key
```bash
source ~/.bashrc
curl -s "https://generativelanguage.googleapis.com/v1beta/models?key=${GEMINI_API_KEY}" | grep -q '"models"' && echo "Gemini API key is valid" || echo "Gemini API key is invalid or has no access"
```