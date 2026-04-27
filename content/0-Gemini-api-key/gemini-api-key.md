GEMINI API KEY

```bash
export GEMINI_API_KEY=
curl -s "https://generativelanguage.googleapis.com/v1beta/models?key=${GEMINI_API_KEY}" \
  | grep -q '"models"' \
  && echo "Gemini API key is valid" \
  || echo "Gemini API key is invalid or has no access"
```