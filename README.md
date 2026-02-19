<div align="right">
  <a href="README.md">English</a> | <a href="README_ZH.md">中文</a>
</div>

# Koma Skills

> 🧰 A curated collection of AI Skills for specialized domain tasks

---

## 📦 Skills

### 📚 FM Knowledge Guide

> 🎓 AI Skill to help bank developers and business analysts master financial market knowledge

![Document Viewer UI](sample/img/ui.png)

| Feature | Description |
|---------|-------------|
| 📚 **Comprehensive Coverage** | Covers derivatives, collateral management, trade lifecycle, regulatory frameworks |
| 🎯 **Role-Adaptive** | Automatically adjusts content depth based on user role (Developer/BA/Operations) |
| 📊 **Mermaid Diagrams** | Auto-generates flowcharts, sequence diagrams, ER diagrams |
| 🌐 **Built-in Doc Viewer** | Dark theme web UI with Markdown rendering and diagram support |
| 📁 **Auto-Archive** | Generated docs automatically saved to `docs/fm-guide/` directory |

**Supported Topics**: Derivatives, Collateral Management, Trade Lifecycle, Messaging Standards, Regulations, Risk Management

**Usage**:

```bash
# Workflow trigger (recommended)
@/fm-knowledge-guider margin call

# Skill installation
npx skills add Kooooooma/skills@fm-knowledge-guide -g
```

<details>
<summary>📂 Project Structure</summary>

```
fm-knowledge-guide/
├── SKILL.md                 # Skill entry point and workflow definition
├── references/              # Financial domain reference materials
│   ├── collateral-management.md
│   ├── derivatives-basics.md
│   ├── trade-lifecycle.md
│   ├── messaging-standards.md
│   ├── regulatory-framework.md
│   ├── risk-management.md
│   ├── market-data.md
│   ├── domain-overview.md
│   └── glossary.md
└── templates/
    ├── learning-guide-template.md  # Document generation template
    └── viewer.html                 # Web viewer template
```

</details>

---

### 🛡️ Code Security Scanner

> 🔍 AI Skill to scan code repositories for security threats — data exfiltration, backdoors, malicious code, supply-chain attacks

| Feature | Description |
|---------|-------------|
| 🔴 **Data Exfiltration** | Detects credentials/tokens being sent to external servers |
| 🔴 **Backdoor Detection** | Finds hidden endpoints, reverse shells, undocumented remote access |
| 🔴 **Malicious Code** | Identifies `eval()`, obfuscation, `postinstall` exploits |
| 🟡 **Dependency Risks** | Audits npm dependencies for typosquatting, vulnerable packages |
| 🟡 **Filesystem Risks** | Detects `~/.ssh/`, browser cookies, credential store access |

**Optimized for**: TypeScript / JavaScript / Node.js projects

**Usage**:

```bash
# Skill installation
npx skills add Kooooooma/skills@code-security-scanner -g
```

**Example prompts**:

| User Input | Scan Scope |
|------------|------------|
| `scan this project for security threats` | Full 5-phase audit |
| `check for backdoors in this codebase` | Backdoor detection focus |
| `audit npm dependencies` | Dependency chain analysis |
| `check for data exfiltration` | Credential leak detection |

<details>
<summary>📂 Project Structure</summary>

```
code-security-scanner/
├── SKILL.md                 # Skill entry point and scan workflow
└── references/              # Detection pattern references
    ├── data-exfiltration.md     # 🔴 Credential/token leak patterns
    ├── backdoor-detection.md    # 🔴 Reverse shell, hidden endpoint patterns
    ├── malicious-code-patterns.md # 🔴 eval, obfuscation, postinstall exploits
    ├── dependency-risks.md      # 🟡 Supply-chain attack patterns
    └── filesystem-risks.md      # 🟡 Sensitive file access patterns
```

</details>

---

## ⭐ Star History

[![Star History Chart](https://api.star-history.com/svg?repos=Kooooooma/skills&type=Date)](https://star-history.com/#Kooooooma/skills&Date)

---

## 📄 License

Apache License 2.0
