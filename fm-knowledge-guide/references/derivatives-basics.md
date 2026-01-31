# Derivatives Basics

## What Are Derivatives?

Derivatives are financial contracts whose value is derived from an underlying asset, index, or rate. They are used for:

- **Hedging**: Reducing risk exposure
- **Speculation**: Profiting from price movements
- **Arbitrage**: Exploiting price differences

---

## Types of Derivatives

### 1. Swaps

Exchange of cash flows between two parties over time.

#### Interest Rate Swap (IRS)
Exchange fixed rate for floating rate payments.

```
Party A ────── Fixed Rate (e.g., 3%) ──────▶ Party B
Party A ◀───── Floating Rate (e.g., SOFR + 50bps) ───── Party B
```

**Key Terms:**
- **Notional**: Principal amount (not exchanged)
- **Fixed Leg**: Fixed rate payments
- **Floating Leg**: Payments based on reference rate
- **Payment Frequency**: Monthly, quarterly, semi-annual

#### Cross Currency Swap (CCS)
Exchange of principal and interest in different currencies.

**Key Difference from IRS**: Principal IS exchanged at start and maturity.

#### FX Swap
Simultaneous spot and forward FX transaction.

```
Day 1 (Spot):   Buy USD / Sell EUR
Day N (Forward): Sell USD / Buy EUR
```

### 2. Options

Right (not obligation) to buy or sell at a specified price.

| Type | Right | Premium |
|------|-------|---------|
| Call | Buy at strike price | Buyer pays |
| Put | Sell at strike price | Buyer pays |

**Key Terms:**
- **Strike Price**: Agreed exercise price
- **Premium**: Cost to buy the option
- **Expiry**: Last date to exercise
- **In/Out of the Money**: Profitable vs unprofitable to exercise

### 3. Futures

Standardized contracts to buy/sell at future date (exchange-traded).

**Characteristics:**
- Daily mark-to-market
- Margin requirements
- Standardized terms
- CCP cleared

### 4. Forwards

Customized contracts to buy/sell at future date (OTC).

**Characteristics:**
- Flexible terms
- Counterparty risk
- Settled at maturity
- No daily margining (unless CSA)

---

## OTC vs Exchange-Traded

| Aspect | OTC | Exchange-Traded |
|--------|-----|-----------------|
| Customization | High | Standardized |
| Counterparty Risk | Yes (unless cleared) | CCP managed |
| Transparency | Lower | Higher |
| Liquidity | Varies | Generally higher |
| Margining | Bilateral (CSA) | Daily by CCP |
| Examples | IRS, CCS, FX Forwards | Futures, Listed Options |

---

## Valuation Concepts

### Mark-to-Market (MTM)

Current market value of a position.

```
MTM = Present Value of Future Cash Flows
```

Calculated using:
- Discount curves
- Forward rates
- Volatility surfaces (for options)

### Net Present Value (NPV)

Sum of discounted future cash flows minus initial investment.

### Greeks (for Options)

| Greek | Measures | Use |
|-------|----------|-----|
| Delta | Price sensitivity to underlying | Hedging |
| Gamma | Delta sensitivity to underlying | Risk assessment |
| Vega | Price sensitivity to volatility | Vol trading |
| Theta | Time decay | Position management |
| Rho | Interest rate sensitivity | Rate risk |

---

## System Implementation Considerations

### Pricing Engines
- Integration with market data feeds
- Curve building (discount, forward, basis)
- Model calibration
- Greeks calculation

### Trade Capture
- Product templates
- Trade booking workflows
- Amendment/cancellation handling
- Trade versioning

### Lifecycle Events
- Roll dates
- Reset/fixing dates
- Payment generation
- Early termination

### Integration Points
- Collateral systems (for MTM-based margin)
- Risk systems (Greeks, VaR)
- Regulatory reporting (EMIR, Dodd-Frank)
