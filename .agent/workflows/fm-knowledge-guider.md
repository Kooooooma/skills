---
description: Trigger fm-knowledge-guide skill to help users learn financial market concepts.
---

# Financial Market Knowledge Guide Workflow

This workflow triggers the `fm-knowledge-guide` skill to help bank developers and BAs understand financial market business knowledge.

## Trigger Keywords

- **Derivatives**: swaps, options, futures, forwards, IRS, CCS, FX swap
- **Collateral**: CSA, margin call, VM, IM, collateral, haircut, threshold
- **Trade Lifecycle**: confirmation, clearing, settlement, DVP, FOP, SSI
- **Products**: FX, fixed income, bonds, equities, repos
- **Regulations**: EMIR, Dodd-Frank, MiFID, UMR, Basel

## Workflow Steps

### Step 1: Check Skill Installation

Verify the skill is available:

```bash
npx skills list | grep fm-knowledge-guide
```

If not installed, guide the user:
```
npx skills add Kooooooma/skills@fm-knowledge-guide -g -y
```

### Step 2: Load and Execute Skill

**MANDATORY**: Read and follow the skill's complete workflow:

1. Read: `fm-knowledge-guide/SKILL.md`
2. Execute the **entire workflow** as defined in the skill

> ⚠️ Do NOT duplicate any logic here. The skill is the single source of truth for all steps and procedures.

## Example Interactions

| User Request | Action |
|--------------|--------|
| "Explain margin calls" | Load skill → Execute workflow |
| "VM vs IM difference" | Load skill → Execute workflow |
| "Generate derivatives guide" | Load skill → Execute workflow |