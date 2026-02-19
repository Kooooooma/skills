# Sensitive File System Access Patterns

Patterns for detecting code that reads sensitive local files or credential stores.

## Grep Patterns

```bash
# SSH key access
grep -rnE "(\.ssh|id_rsa|id_ed25519|known_hosts|authorized_keys)" --include="*.ts" --include="*.js"

# AWS credentials
grep -rnE "(\.aws/credentials|\.aws/config)" --include="*.ts" --include="*.js"

# Browser profile / cookie access
grep -rniE "(chrome|firefox|chromium|edge|safari|brave).*(cookie|profile|login|password|local.?state)" --include="*.ts" --include="*.js"

# .env files outside project
grep -rnE "readFile.*\.env|readFileSync.*\.env" --include="*.ts" --include="*.js"
grep -rnE "path\.(join|resolve).*\.\." --include="*.ts" --include="*.js" | grep -i env

# OS keychain / credential manager
grep -rniE "(keytar|keychain|credential.?manager|secret.?service|libsecret|wincred)" --include="*.ts" --include="*.js"

# GnuPG keys
grep -rnE "(\.gnupg|gpg-agent|secring)" --include="*.ts" --include="*.js"

# Git credentials
grep -rnE "(\.git-credentials|\.gitconfig|credential\.helper)" --include="*.ts" --include="*.js"

# Home directory traversal
grep -rnE "(os\.homedir|process\.env\.HOME|process\.env\.USERPROFILE)" --include="*.ts" --include="*.js"
```

## Sensitive Paths to Monitor

### 🔴 Critical — Cryptographic Keys & Credentials

```
~/.ssh/id_rsa          # SSH private key
~/.ssh/id_ed25519      # SSH private key (Ed25519)
~/.ssh/known_hosts     # SSH known hosts
~/.aws/credentials     # AWS access keys
~/.aws/config          # AWS region/profile config
~/.gnupg/              # GPG private keys
~/.git-credentials     # Git stored passwords
~/.npmrc               # npm auth tokens
~/.docker/config.json  # Docker registry credentials
~/.kube/config         # Kubernetes cluster credentials
```

### 🟡 Medium — Browser Data & Application Secrets

```
# Chrome (Windows)
%LOCALAPPDATA%\Google\Chrome\User Data\Default\Cookies
%LOCALAPPDATA%\Google\Chrome\User Data\Default\Login Data
%LOCALAPPDATA%\Google\Chrome\User Data\Local State

# Chrome (macOS)
~/Library/Application Support/Google/Chrome/Default/Cookies

# Chrome (Linux)
~/.config/google-chrome/Default/Cookies

# Firefox
~/.mozilla/firefox/*.default/cookies.sqlite
~/.mozilla/firefox/*.default/logins.json

# Edge
%LOCALAPPDATA%\Microsoft\Edge\User Data\Default\Cookies
```

### 🟡 Medium — Application Config Files

```
~/.env                 # Global environment variables
~/.bashrc, ~/.zshrc    # Shell configs (may contain exports)
~/.netrc               # FTP/HTTP credentials
~/.pgpass              # PostgreSQL passwords
~/.my.cnf              # MySQL credentials
~/.config/             # XDG config directory
```

## Suspicious Patterns

### 1. SSH Key Theft

```javascript
// 🔴 CRITICAL — Reading SSH private key
const fs = require("fs");
const path = require("path");
const sshKey = fs.readFileSync(
  path.join(os.homedir(), ".ssh", "id_rsa"), "utf8"
);
// ... then sent externally
```

### 2. Browser Cookie Harvesting

```javascript
// 🟡 MEDIUM — Reading Chrome cookies database
const cookiePath = path.join(
  process.env.LOCALAPPDATA,
  "Google/Chrome/User Data/Default/Cookies"
);
const db = new Database(cookiePath);
const cookies = db.prepare("SELECT * FROM cookies").all();
```

### 3. Environment File Traversal

```javascript
// 🟡 MEDIUM — Reading .env from parent directories
let dir = process.cwd();
while (dir !== path.dirname(dir)) {
  const envPath = path.join(dir, ".env");
  if (fs.existsSync(envPath)) {
    const content = fs.readFileSync(envPath, "utf8");
    // potentially collecting secrets from unrelated projects
  }
  dir = path.dirname(dir);
}
```

### 4. OS Keychain Access

```javascript
// 🟡 MEDIUM — Accessing system keychain
const keytar = require("keytar");
const password = await keytar.getPassword("service", "account");
// Legitimate use: app's own credentials
// Suspicious if: accessing other apps' stored passwords
```

## Path Traversal Detection

Flag any file read operations that:
1. Use `..` to go above the project root
2. Reference `os.homedir()` or `process.env.HOME`
3. Access absolute paths outside the project directory
4. Use `glob` patterns that could match sensitive directories
