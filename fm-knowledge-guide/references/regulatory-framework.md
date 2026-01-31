# Regulatory Framework

## Overview

Financial markets are heavily regulated to ensure stability, transparency, and investor protection. Understanding key regulations is essential for building compliant banking systems.

---

## Key Regulations

### EMIR (European Market Infrastructure Regulation)

**Scope**: EU derivatives regulation

**Key Requirements**:
- **Clearing**: Mandatory CCP clearing for standardized OTC derivatives
- **Reporting**: All derivatives reported to trade repositories
- **Risk Mitigation**: Timely confirmation, portfolio reconciliation, dispute resolution
- **Margining**: Bilateral margin rules for uncleared derivatives

**System Impact**:
- Trade repository connectivity
- LEI (Legal Entity Identifier) management
- Confirmation deadline tracking
- Margin calculation and exchange

---

### Dodd-Frank Act

**Scope**: US financial reform (Title VII covers derivatives)

**Key Requirements**:
- **Clearing**: Mandatory clearing via DCOs (Derivatives Clearing Organizations)
- **Trading**: Required trading on SEFs (Swap Execution Facilities)
- **Reporting**: Real-time public reporting, SDR (Swap Data Repository) reporting
- **Margin**: Uncleared swap margin rules

**System Impact**:
- SEF connectivity
- Real-time trade reporting
- USI/UTI (Unique Swap/Transaction Identifier) generation
- Cross-border compliance rules

---

### MiFID II / MiFIR (Markets in Financial Instruments)

**Scope**: EU markets regulation

**Key Requirements**:
- **Transparency**: Pre and post-trade transparency
- **Best Execution**: Prove best execution for clients
- **Transaction Reporting**: Detailed trade reporting
- **Product Governance**: Product approval processes

**System Impact**:
- Transaction reporting (ARM connectivity)
- Best execution analysis
- Client classification
- Recording of communications

---

### Basel III / IV

**Scope**: Global banking capital requirements

**Key Areas**:
- **Capital**: Minimum capital ratios
- **Leverage**: Leverage ratio limits
- **Liquidity**: LCR (Liquidity Coverage Ratio), NSFR (Net Stable Funding Ratio)
- **CVA**: Credit Valuation Adjustment capital charge

**System Impact**:
- Capital calculation engines
- Exposure calculation (SA-CCR)
- Risk-weighted asset calculation
- Regulatory reporting (COREP)

---

### UMR (Uncleared Margin Rules)

**Scope**: Margin for non-centrally cleared derivatives

**Timeline** (phased implementation):
- Phase 1-5: Largest institutions (2016-2020)
- Phase 6: AANA > €8bn (2022)

**Requirements**:
- **VM**: Mandatory variation margin exchange
- **IM**: Initial margin calculation (ISDA SIMM or Grid)
- **Segregation**: IM held at third-party custodian
- **Documentation**: Compliant CSA agreements

**System Impact**:
- SIMM calculation engine
- IM/VM workflow separation
- Tri-party custody integration
- Threshold monitoring

---

### SFTR (Securities Financing Transactions Regulation)

**Scope**: EU SFT (repos, securities lending) reporting

**Requirements**:
- Daily reporting to trade repositories
- Detailed counterparty and collateral information
- LEI, UTI, and collateral reuse data

**System Impact**:
- SFT trade repository connectivity
- Collateral tracking
- UTI generation and matching

---

## Key Identifiers

| Identifier | Purpose | Example |
|------------|---------|---------|
| LEI | Legal Entity Identifier | 5493000IBP32UQZ0KL24 |
| UTI | Unique Transaction Identifier | 1034500E6ZVPRG13PV57221210000001 |
| USI | Unique Swap Identifier (US) | 1030MXTP01S000000001 |
| ISIN | Securities identifier | US0378331005 |
| CFI | Classification of Financial Instruments | OPASPS |

---

## Reporting Obligations

### What to Report

| Regulation | Products | Repository |
|------------|----------|------------|
| EMIR | All derivatives | DTCC, Regis-TR, etc. |
| Dodd-Frank | Swaps, SBS | DTCC, CME SDR, etc. |
| MiFIR | All MiFID instruments | ARMs |
| SFTR | Repos, sec lending | Trade repositories |

### Reporting Frequency

- **Same day**: Most derivative reports
- **T+1**: Some equity/bond transactions
- **Real-time**: US swap public reporting

---

## Cross-Border Considerations

### Substituted Compliance
Recognition of equivalent foreign regulations.

### Extra-Territoriality
- US rules apply if US person involved
- EU rules apply to EU entities and non-EU dealing with EU

### Multiple Reporting
Same trade may need to be reported under multiple regimes.

---

## System Requirements

### Regulatory Reporting Engine
- Data aggregation from trading systems
- Regulatory format transformation
- Submission to repositories/regulators
- Reconciliation and error handling

### Reference Data
- LEI management
- Product classification
- Jurisdiction determination
- Exemption tracking

### Audit Trail
- Complete trade history
- Amendment tracking
- Reporting status
- Regulatory queries response
