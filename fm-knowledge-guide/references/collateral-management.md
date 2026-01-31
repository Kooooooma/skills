# Collateral Management

## Overview

Collateral management is the process of exchanging assets to mitigate credit risk in financial transactions. It's governed by legal agreements and is critical for OTC derivatives.

---

## Legal Framework

### ISDA Master Agreement
Standard contract governing OTC derivative transactions between parties.

### Credit Support Annex (CSA)
Supplement to ISDA Master Agreement that defines collateral terms:

- **Eligible Collateral**: What assets can be posted
- **Haircuts**: Valuation adjustments for collateral
- **Thresholds**: Minimum exposure before margin required
- **Minimum Transfer Amount (MTA)**: Smallest amount to transfer
- **Rounding**: How amounts are rounded
- **Independent Amount (IA)**: Fixed collateral amount (like IM)

---

## Margin Types

### Variation Margin (VM)

Covers **current exposure** (mark-to-market changes).

**Purpose**: Protect against today's price movements

**Calculation**:
```
VM Call = MTM - Collateral Held + Threshold + MTA
```

**Characteristics**:
- Exchanged daily
- Two-way (both parties may post)
- Based on MTM of portfolio

### Initial Margin (IM)

Covers **potential future exposure** over a close-out period.

**Purpose**: Protect against potential losses during default

**Calculation Methods**:
- **ISDA SIMM**: Standardized model (common for bilateral)
- **CCP Model**: Proprietary models (for cleared trades)

**Characteristics**:
- Posted upfront and adjusted periodically
- Typically segregated at third-party custodian
- Required by UMR (Uncleared Margin Rules)

---

## Margin Call Lifecycle

```
┌─────────────────────────────────────────────────────────────────────┐
│                        MARGIN CALL LIFECYCLE                        │
└─────────────────────────────────────────────────────────────────────┘

  ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
  │  1. CALC │    │ 2. CALL  │    │ 3. AGREE │    │ 4. SETTLE│
  │          │───▶│          │───▶│          │───▶│          │
  │  MTM     │    │  Issue   │    │  Confirm │    │  Transfer│
  └──────────┘    └──────────┘    └──────────┘    └──────────┘
       │               │               │               │
   Calculate       Send Call       Counterparty    Collateral
   Exposure        to Cpty        Agrees/Disputes   Moves
```

### Step 1: Calculation
- Calculate portfolio MTM
- Determine required collateral
- Apply thresholds, MTA, rounding

### Step 2: Call Issuance
- **Margin Call (MC)**: Demand for collateral (deficit)
- **Return Amount**: Return of excess collateral (surplus)

### Step 3: Agreement
- Counterparty agrees to amount
- Or disputes and provides their calculation
- Resolution via agreed methodology

### Step 4: Settlement
- **Pledge**: Transfer of collateral ownership
- **Instruction**: Sending settlement instructions
- Via tri-party agent or direct transfer

---

## Call Direction

| Scenario | My Position | Action |
|----------|-------------|--------|
| I owe money (MTM negative) | Out of the Money | I post collateral |
| They owe money (MTM positive) | In the Money | I receive collateral |

---

## Eligible Collateral

Typical eligible collateral types:

| Type | Haircut Range | Notes |
|------|---------------|-------|
| Cash (USD, EUR, GBP) | 0% | Most common |
| Government Bonds (G7) | 0.5% - 5% | Depending on maturity |
| Corporate Bonds (IG) | 5% - 15% | Credit quality dependent |
| Equities | 15% - 25% | Less common |

### Haircut
Discount applied to collateral value to account for potential price volatility.

```
Collateral Value = Market Value × (1 - Haircut)
```

---

## Substitution

Process of replacing posted collateral with different eligible assets.

**Steps**:
1. Request substitution
2. Agreement on replacement assets
3. Simultaneous exchange

---

## Interest on Collateral

### Price Alignment Interest (PAI)
Interest paid/received on cash collateral.

- Reflects cost/benefit of holding cash
- Usually tied to overnight rates (SOFR, €STR)
- Reduces funding costs

---

## System Implementation

### Key Calculations
- MTM aggregation by CSA
- Threshold and MTA application
- Haircut application
- Call amount rounding

### Integration Points
- Pricing systems (MTM feeds)
- Position keeping (trade data)
- SSI database (settlement instructions)
- Custody systems (collateral inventory)

### Workflow Automation
- Automatic call generation
- Dispute management
- Settlement instruction generation
- Reconciliation with counterparties

### Reporting
- Collateral inventory
- Exposure reports
- Dispute aging
- Regulatory reporting (IM disclosure)
