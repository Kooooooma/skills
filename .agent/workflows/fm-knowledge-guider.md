---
description: Trigger fm-knowledge-guide skill to help users learn financial market concepts. Use when users ask about derivatives, collateral, margin calls, trade lifecycle, FX, fixed income, or regulatory topics.
---

# Financial Market Knowledge Guide Workflow

This workflow triggers the `fm-knowledge-guide` skill to help bank developers and BAs understand financial market business knowledge.

## Prerequisites

Before using this workflow, ensure the `fm-knowledge-guide` skill is installed:

```bash
# Check if skill is installed
npx skills list | grep fm-knowledge-guide

# If not installed, install it:
npx skills add Kooooooma/skills@fm-knowledge-guide -g -y
```

## Trigger Keywords

This workflow activates when users mention:

- **Derivatives**: swaps, options, futures, forwards, IRS, CCS, FX swap
- **Collateral**: CSA, margin call, VM, IM, collateral, haircut, threshold
- **Trade Lifecycle**: confirmation, clearing, settlement, DVP, FOP, SSI
- **Products**: FX, fixed income, bonds, equities, repos
- **Regulations**: EMIR, Dodd-Frank, MiFID, UMR, Basel

## Workflow Steps

### Step 1: Check Skill Installation

First, verify the skill is available. If not installed, guide the user:

```
The fm-knowledge-guide skill is required but not installed.

To install, run:
npx skills add Kooooooma/skills@fm-knowledge-guide -g -y

Then try your question again.
```

### Step 2: Identify the Topic Domain

Determine which reference file to consult:

| User Keywords | Reference File |
|--------------|----------------|
| asset class, FX, equity, bond | `references/domain-overview.md` |
| swap, option, future, forward, IRS | `references/derivatives-basics.md` |
| CSA, margin, collateral, VM, IM | `references/collateral-management.md` |
| confirmation, clearing, settlement | `references/trade-lifecycle.md` |
| EMIR, Dodd-Frank, UMR, regulation | `references/regulatory-framework.md` |
| term definition, glossary | `references/glossary.md` |

### Step 3: Load and Apply Skill

Read the SKILL.md and relevant reference files:

```
Skill location: fm-knowledge-guide/SKILL.md
References: fm-knowledge-guide/references/
```

### Step 4: Generate Response

Follow the skill's workflow to:
1. Explain the "Why" (business purpose)
2. Define the "What" (concepts)
3. Show the "How" (examples)
4. Connect to IT systems (implementation)

### Step 5: Save Learning Document (REQUIRED)

**Always save the generated learning document** to the project docs directory:
```
docs/fm-guide/[topic-name]-guide.md
```

**Examples**:
- Margin Call → `docs/fm-guide/margin-call-guide.md`
- Acadia → `docs/fm-guide/acadia-guide.md`
- CSA → `docs/fm-guide/csa-guide.md`

### Step 6: Ensure Document Viewer Exists (REQUIRED)

**Check and copy viewer.html from skill templates if not exists:**

```powershell
# Check and copy viewer.html if not exists
if (!(Test-Path "docs/fm-guide/viewer.html")) {
    Copy-Item "<skill-path>/fm-knowledge-guide/templates/viewer.html" "docs/fm-guide/viewer.html"
}
```

### Step 7: Launch Document Viewer (OPTIONAL)

After generating documents, launch a local HTTP server to view all documents in a web browser.

**Command**:
```bash
// turbo
npx -y http-server docs/fm-guide -p 0 -o /viewer.html
```

**Options Explained**:
- `-p 0`: Use random available port
- `-o /viewer.html`: Auto-open browser to viewer.html

**Document Viewer Features**:
- Dynamically lists all .md files in the directory
- Left sidebar: Lists all generated learning guides
- Right panel: Renders markdown content with Mermaid diagrams
- Dark theme with modern UI

## Example Interactions

**User**: "Explain how margin calls work"
→ Load `collateral-management.md`, generate detailed margin call explanation

**User**: "What's the difference between VM and IM?"
→ Load `collateral-management.md`, compare Variation Margin vs Initial Margin

**User**: "Help me understand the trade lifecycle"
→ Load `trade-lifecycle.md`, explain execution to settlement flow

**User**: "Generate a learning doc about derivatives"
→ Load `derivatives-basics.md`, create comprehensive guide in `docs/fm-guide/`

**User**: "View my generated docs"
→ Launch HTTP server with `npx -y http-server docs/fm-guide -p 0 -o /viewer.html`



