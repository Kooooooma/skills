# Dependency Chain Risk Patterns

Patterns for identifying supply-chain attacks through npm dependencies.

## Automated Checks

```bash
# Run npm audit for known vulnerabilities
npm audit --json

# List all postinstall scripts in dependencies
npm ls --json | grep -i postinstall

# Check for unpinned versions
grep -E "\"\\*\"|\"latest\"|\">=|\">" package.json

# Find recently published dependencies (check npm registry)
# For each dep: npm view <package> time --json
```

## Risk Categories

### 1. Typosquatting Packages

Attackers publish packages with names similar to popular ones:

| Legitimate | Typosquat Examples |
|-----------|-------------------|
| `lodash` | `lodahs`, `lodash-utils`, `l0dash` |
| `express` | `expres`, `express-js`, `expresss` |
| `react` | `raect`, `react-core`, `reactjs` |
| `chalk` | `chalkk`, `chak`, `chal-k` |
| `axios` | `axois`, `axi0s`, `axio` |
| `moment` | `momnet`, `moement` |

**Detection**: Compare all dependency names against a known-good list. Flag any package whose name differs by 1-2 characters from a top-1000 npm package.

### 2. Suspicious postinstall Hooks in Dependencies

```bash
# Search all node_modules for postinstall scripts
find node_modules -name "package.json" -maxdepth 3 -exec grep -l "postinstall" {} \;

# Check what postinstall scripts do
for pkg in $(find node_modules -name "package.json" -maxdepth 3 -exec grep -l "postinstall" {} \;); do
  echo "=== $pkg ==="
  cat "$pkg" | grep -A2 "postinstall"
done
```

### 3. Version Pinning Issues

Flag these version patterns in `package.json`:

```json
{
  "dependencies": {
    "risky-1": "*",           // 🔴 Any version
    "risky-2": "latest",      // 🔴 Always latest
    "risky-3": ">=1.0.0",     // 🟡 No upper bound
    "risky-4": "^1.0.0",      // 🟡 Allows minor/patch bumps
    "safe": "1.2.3"           // ✅ Pinned exact version
  }
}
```

### 4. Package Health Indicators

Flag packages that exhibit:

| Indicator | Risk Level | Check |
|-----------|-----------|-------|
| < 100 weekly downloads | 🔴 High | `npm view <pkg> --json` |
| Created within last 30 days | 🔴 High | Check `time.created` field |
| Single maintainer + new account | 🟡 Medium | Check npm profile |
| No repository URL | 🟡 Medium | Missing `repository` in package.json |
| Obfuscated/minified source in npm | 🔴 High | Check `node_modules/<pkg>/` contents |
| Excessive permissions (network, fs) | 🟡 Medium | Analyze code imports |

### 5. Lockfile Integrity

```bash
# Verify lockfile integrity
npm ci --dry-run

# Check for lockfile manipulation — compare resolved URLs
grep -E "\"resolved\":" package-lock.json | sort | uniq -c | sort -rn

# Flag non-registry URLs in lockfile
grep -E "\"resolved\":" package-lock.json | grep -v "registry.npmjs.org"
```

## Known Malicious Package Patterns

Historical supply-chain attacks to reference:

- **event-stream** (2018) — Injected cryptocurrency-stealing code via `flatmap-stream` dependency
- **ua-parser-js** (2021) — Maintainer account compromised, cryptominer injected
- **colors/faker** (2022) — Maintainer sabotaged own packages with infinite loop
- **node-ipc** (2022) — Maintainer added geopolitical protest code deleting files
- **eslint-scope** (2018) — Stolen npm token used to publish malicious version
