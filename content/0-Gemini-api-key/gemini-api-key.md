GEMINI API KEY

```bash
echo "Enter Google Gemini API key:"
stty -echo
read -r GEMINI_API_KEY
stty echo
echo

export GEMINI_API_KEY

curl -s "https://generativelanguage.googleapis.com/v1beta/models?key=${GEMINI_API_KEY}" \
  | grep -q '"models"' \
  && echo "Gemini API key is valid" \
  || echo "Gemini API key is invalid or has no access"
```