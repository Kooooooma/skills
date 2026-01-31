# Learning Guide Document Template

> **This template defines the structure for all financial market learning guides.**

---

## Document Structure

```markdown
# [Topic] Learning Guide

## Overview
- What this topic is and why it matters
- Business context and industry background  
- Target audience (roles/teams who need this)

## Table of Contents
<!-- Generate links based on actual sections included -->
- [Section Name](#section-name)
- [Another Section](#another-section)
  - [Subsection](#subsection)
...
```

---

## Required Sections (ALWAYS include)

### 1. Core Concepts

```markdown
## Core Concepts

> **TL;DR**: [One-sentence summary of the core concept for quick scanning]

### Terminology & Definitions
### Conceptual Model (with Mermaid diagram)

> ⚠️ **Watch Out**: [Topic-specific common mistake to avoid]
```

### 2. System Implementation

```markdown
## System Implementation

> **TL;DR**: [One-sentence summary of how this is implemented]

### Data Model (with ER diagram)
### Field Mapping (Business → System)
| Business Term | Database Field | API/Message Field |
|---------------|----------------|-------------------|
| [e.g., MTM] | trade.valuation.mtm_amount | MT305:Field77H |

### System Integration Points
### Processing Logic
```

### 3. Real-World Scenarios

```markdown
## Real-World Scenarios
### Scenario 1: [Normal Case]
### Scenario 2: [Edge Case]  
### Scenario 3: [Exception/Dispute]
```

---

## Dynamic Sections (include when relevant)

### Lifecycle
Include for process/flow topics (margin call, trade lifecycle, settlement)

```markdown
## [Topic] Lifecycle
### End-to-End Flow (with sequence diagram)
### Phase Breakdown (Trigger → Actions → Output → Exceptions)
```

### Comparisons
Include when explaining differences (VM vs IM, Pledge vs Title Transfer)

```markdown
## [Concept A] vs [Concept B]
| Aspect | A | B |
|--------|---|---|
| Purpose | ... | ... |
```

### Calculations
Include for topics with formulas (margin calculation, haircut, interest)

```markdown
## Calculation & Formulas
- Formula with explanation
- Worked example with numbers
- Common pitfalls
```

### Handling Scenarios
Include for operational topics

```markdown
## Handling Scenarios
### Normal Processing
### Exception Handling
### Dispute Resolution
```

### Best Practices
Include for implementation guidance

```markdown
## Best Practices & Pitfalls
### Do's
### Don'ts
```

---

## References Section (ALWAYS include with REAL links)

```markdown
## References & Resources
<!-- Search and include actual official URLs relevant to the topic -->
```

**Link to real external resources based on topic:**

| Topic Area | Example Resources |
|------------|-------------------|
| Margin/Collateral | [ISDA](https://www.isda.org/), [UMR Guidelines](https://www.bis.org/bcbs/publ/d317.htm) |
| Trading | [FpML](https://www.fpml.org/), [FIX Protocol](https://www.fixtrading.org/) |
| Regulatory | [EMIR](https://eur-lex.europa.eu/), [Dodd-Frank](https://www.cftc.gov/), [Basel](https://www.bis.org/) |
| Platforms | [Acadiasoft](https://www.acadiasoft.com/), [DTCC](https://www.dtcc.com/) |

---

## Appendix (ALWAYS include with REAL data)

```markdown
## Appendix
### Glossary
### Reference Data
```

---

## Section Selection Guide

| Topic Type | Required | Recommended |
|------------|----------|-------------|
| Process/Flow | Overview, Core, Scenarios | Lifecycle, Handling |
| Concept | Overview, Core, Implementation | Comparisons, Best Practices |
| Calculation | Overview, Core, Calculations | Scenarios |
| Regulatory | Overview, Core | Best Practices, References |
