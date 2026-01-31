# Trade Lifecycle

## Overview

The trade lifecycle encompasses all stages from trade initiation to final settlement. Understanding this flow is essential for building trading systems.

---

## Lifecycle Stages

```
┌─────────────┐   ┌─────────────┐   ┌─────────────┐   ┌─────────────┐   ┌─────────────┐
│  PRE-TRADE  │──▶│  EXECUTION  │──▶│ POST-TRADE  │──▶│  CLEARING   │──▶│ SETTLEMENT  │
└─────────────┘   └─────────────┘   └─────────────┘   └─────────────┘   └─────────────┘
     │                  │                  │                  │                  │
  Pricing            Booking          Confirmation        Netting          Cash/Sec
  Credit Check       Capture          Matching            Novation         Transfer
  Quote              Validation       Amendments          Margin           Custody
```

---

## Stage 1: Pre-Trade

### Pricing
- Obtain market data
- Calculate fair value
- Apply spreads/margins

### Credit Check
- Verify counterparty credit limits
- Check exposure vs limits
- Approve or reject

### RFQ (Request for Quote)
- Counterparty requests price
- Dealer provides indicative/firm quote
- Negotiation if needed

---

## Stage 2: Execution

### Trade Capture
- Record trade details
- Assign unique trade ID
- Book to correct book/portfolio

### Trade Validation
- Economic validation
- Regulatory validation
- Limit validation

### Booking
- Create trade record in system
- Generate event calendar
- Trigger downstream processes

---

## Stage 3: Post-Trade

### Confirmation
Formal agreement of trade terms between counterparties.

**Methods**:
- **Electronic**: MarkitWire, DTCC, Bloomberg
- **Paper**: SWIFT MT300, fax (legacy)

**Matching**:
- Automatic matching of key economic terms
- Exception handling for mismatches

### Trade Amendments
- Corrections to trade terms
- Version control
- Audit trail

### Affirmation
Buy-side acknowledgment of trade details (mainly equities/fixed income).

---

## Stage 4: Clearing

### Bilateral Clearing
Direct relationship between two parties.
- Counterparty credit risk remains
- Manual netting
- CSA-based margining

### CCP Clearing
Central Counterparty becomes buyer to seller and seller to buyer.

**Benefits**:
- Reduced counterparty risk
- Multilateral netting
- Standardized margining
- Default management

**Process**:
```
Party A ◀───▶ CCP ◀───▶ Party B

Original: A trades with B
Cleared:  A trades with CCP, B trades with CCP
```

### Netting
Offsetting multiple obligations to reduce settlement amounts.

**Types**:
- **Payment Netting**: Net cash flows on same date/currency
- **Close-out Netting**: All trades netted on default

---

## Stage 5: Settlement

### Settlement Methods

| Method | Description | Use Case |
|--------|-------------|----------|
| DVP (Delivery vs Payment) | Simultaneous exchange | Securities transactions |
| FOP (Free of Payment) | Delivery without payment | Collateral transfers |
| PVP (Payment vs Payment) | Simultaneous payments | FX transactions (CLS) |

### Settlement Instructions

**SSI (Standard Settlement Instructions)**
Pre-agreed instructions for:
- Receiving accounts
- Correspondent banks
- Beneficiary details

### Settlement Cycle

| Product | Standard Cycle |
|---------|---------------|
| FX Spot | T+2 |
| FX Forward | Value Date |
| Equities | T+1 (US), T+2 (EU) |
| Bonds | T+1 to T+2 |
| OTC Derivatives | Per CSA terms |

---

## Key Dates

| Date | Definition |
|------|------------|
| Trade Date (T) | Day trade is executed |
| Value Date | Day trade takes economic effect |
| Settlement Date | Day cash/securities move |
| Maturity Date | Day contract ends |
| Fixing Date | Day rate is observed (floating leg) |
| Payment Date | Day payment is made |

---

## Exception Handling

### Breaks
Discrepancies between systems or counterparties.

**Types**:
- Position breaks
- Cash breaks
- Confirmation breaks

### Disputes
Disagreements on trade terms or valuations.

**Process**:
1. Identify discrepancy
2. Investigate root cause
3. Agree resolution
4. Apply correction

---

## System Considerations

### Real-Time Processing
- Trade capture immediately
- Confirmation matching automated
- Limit monitoring continuous

### End-of-Day Processing
- Settlement instruction generation
- Netting runs
- Regulatory reporting

### Reconciliation
- Internal (front vs back office)
- External (with counterparties, CCPs, custodians)
