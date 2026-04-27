GEMINI API KEY

```bash
read -p "Enter Google Gemini API key: " GEMINI_API_KEY
export GEMINI_API_KEY=$GEMINI_API_KEY
curl -s "https://generativelanguage.googleapis.com/v1beta/models?key=`echo $GEMINI_API_KEY`" | grep -q '"models"' && echo "Gemini API key is valid" || echo "Gemini API key is invalid or has no access"
```