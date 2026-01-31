# Market Data for Developers

## Overview
Market data is the "Fuel" for pricing engines. In a trading system, market data is not just flat numbers; it is structured into complex objects like **Curves**, **Surfaces**, and **Cubes**.

---

## 1. Curves (Yield Curves)
A yield curve represents the relationship between interest rates and time to maturity.

### Structure
A curve is a collection of points (Tenor, Rate) + an Interpolation Method.

| Tenor | Instrument | Rate |
|-------|------------|------|
| 1D | OIS | 3.05% |
| 1M | Deposit | 3.10% |
| 3M | Future | 3.15% |
| 2Y | Swap | 3.50% |
| 10Y | Swap | 4.00% |

### Construction (Bootstrapping)
Raw market quotes (Inputs) must be processed into "Zero Coupon Rates" (Outputs).
1.  **Input**: Select liquid instruments (Cash, Futures, Swaps).
2.  **Strip**: Use a solver to find the zero rate that prices each instrument to par.
3.  **Interpolate**: Define method (Linear, Cubic Spline) for dates between points.

> **Developer Note**: Never store just the "Rate". Store the **Curve ID**, **Snapshot Time**, and **Inputs**.

---

## 2. Volatility Surfaces
Used for pricing Options. Volatility is not constant; it changes with **Strike** (Smile) and **Maturity** (Term Structure).

### The Grid
A 3D surface represented as a grid of volatility points:
- **X-Axis**: Strike Price (or Moneyness)
- **Y-Axis**: Time to Expiry
- **Z-Axis**: Implied Volatility %

### Operations
- **Slicing**: Getting the "Smile" for a specific expiry.
- **Aging**: Rolling the surface forward in time.

---

## 3. Fixings vs Snapshots
### Fixings (The Truth)
Official rates published by benchmarks (e.g., SOFR at 8:00 AM NY).
- **Immutable**: Once published, they never change.
- **Use Case**: Calculating settlement amounts on floating leg swaps.

### Snapshots (The View)
A copy of market data at a specific moment in time.
- **EOD Snapshot**: Taken at 5:00 PM for official P&L.
- **Intraday Snapshot**: Taken every few seconds for real-time risk.

---

## 4. Time Series Management
Handling historical data for Risk/Backtesting.

### Challenges
- **Resets**: Handling corporate actions or rebasing.
- **Holidays**: Filling gaps when markets are closed (Previous vs Linear Fill).
- **Stale Data**: Detecting when a feed has frozen.

### Common Schema
```sql
TABLE market_data_history (
    id UUID,
    instrument_id VARCHAR,
    quote_source VARCHAR,      -- Bloomberg, Reuters
    quote_type VARCHAR,        -- Bid, Ask, Mid
    value DECIMAL,
    capture_time TIMESTAMP,    -- When we got it
    market_time TIMESTAMP      -- When it happened
);
```
