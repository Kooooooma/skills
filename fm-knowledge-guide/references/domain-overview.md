# Financial Market Domain Overview

## Introduction

Financial markets are platforms where buyers and sellers trade financial instruments. For bank IT professionals, understanding these markets is essential for building effective trading, risk management, and settlement systems.

## Asset Classes

### Foreign Exchange (FX)

The largest and most liquid market globally, trading currencies.

**Key Products:**
- **Spot**: Immediate exchange (T+2 settlement)
- **Forward**: Future exchange at agreed rate
- **FX Swap**: Simultaneous spot and forward
- **NDF (Non-Deliverable Forward)**: Cash-settled forward for restricted currencies

**System Considerations:**
- Real-time pricing feeds
- Multi-currency support
- Time zone handling
- Cut-off times by currency

### Fixed Income

Debt instruments that pay fixed or variable interest.

**Key Products:**
- **Government Bonds**: Sovereign debt (Treasuries, Gilts, Bunds)
- **Corporate Bonds**: Company-issued debt
- **Money Market**: Short-term instruments (T-Bills, Commercial Paper, Repos)

**System Considerations:**
- Accrued interest calculations
- Day count conventions (ACT/360, ACT/365, 30/360)
- Coupon schedules
- Bond pricing (clean vs dirty price)

### Derivatives

Contracts deriving value from underlying assets.

**Key Products:**
- **Swaps**: Exchange of cash flows (see `derivatives-basics.md`)
- **Options**: Right to buy/sell at specified price
- **Futures**: Standardized forward contracts (exchange-traded)
- **Forwards**: Customized contracts (OTC)

**System Considerations:**
- Complex pricing models
- Greeks calculation (Delta, Gamma, Vega)
- Mark-to-Market (MTM) valuation
- Collateral management integration

### Equity

Ownership stakes in companies.

**Key Products:**
- **Stocks**: Common and preferred shares
- **ETFs**: Exchange-Traded Funds
- **Equity Derivatives**: Options, futures on stocks/indices

---

## Market Participants

### Sell-Side
- **Investment Banks**: Market making, deal structuring
- **Broker-Dealers**: Trade execution, liquidity provision

### Buy-Side
- **Asset Managers**: Portfolio management
- **Hedge Funds**: Alternative investment strategies
- **Pension Funds**: Long-term investment
- **Insurance Companies**: Liability-driven investment

### Infrastructure
- **Exchanges**: NYSE, LSE, CME (price discovery, standardized trading)
- **CCPs (Central Counterparties)**: LCH, CME Clearing (counterparty risk management)
- **CSDs (Central Securities Depositories)**: Euroclear, DTC (securities custody)
- **Trade Repositories**: DTCC, Regis-TR (regulatory reporting)

---

## Trading Models

### Exchange-Traded
- Standardized contracts
- Central order book
- Transparent pricing
- CCP clearing (reduced counterparty risk)

### Over-The-Counter (OTC)
- Customized terms
- Bilateral negotiation
- Less transparent (historically)
- Bilateral or CCP cleared

---

## Key Market Infrastructure

### SWIFT
Global messaging network for financial transactions (MT/MX messages).

### FIX Protocol
Financial Information eXchange - standard messaging for trade communication.

### Bloomberg/Reuters
Market data providers for pricing, news, analytics.

---

## Front-to-Back Flow

```
┌─────────────┐   ┌─────────────┐   ┌─────────────┐   ┌─────────────┐
│  Front      │   │  Middle     │   │  Back       │   │  External   │
│  Office     │──▶│  Office     │──▶│  Office     │──▶│  Systems    │
└─────────────┘   └─────────────┘   └─────────────┘   └─────────────┘
     │                  │                  │                  │
  Trading            Risk Mgmt         Settlement         Clearing
  Pricing            P&L Calc          Custody            Reporting
  Execution          Compliance        Reconciliation     Reg Filing
```

**Front Office**: Trade execution, pricing, sales
**Middle Office**: Risk management, P&L, compliance
**Back Office**: Settlement, custody, reconciliation
