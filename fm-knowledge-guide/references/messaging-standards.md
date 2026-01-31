# Messaging Standards for Developers

## Overview
In banking, systems talk to each other using strict protocols. As a developer, you don't just "send JSON"; you generate **SWIFT**, **FIX**, or **FpML** messages.

---

## 1. SWIFT (Payments & Confirmations)
The global standard for inter-bank communication.

### MT (Message Type) - Legacy
The "Fin" standard. Strict block format `{1:}{2:}...`.
- **MT103**: Single Customer Credit Transfer (The "Payment").
- **MT202**: Financial Institution Transfer (Bank-to-Bank coverage).
- **MT300**: FX Confirmation.
- **MT305**: Foreign Currency Option Confirmation.
- **MT540-543**: Securities Settlement Instructions.

### MX (XML / ISO 20022) - The Future
The modern XML-based standard replacing MT. Richer data model.
- **pacs.008**: Replaces MT103 (Payments).
- **pacs.009**: Replaces MT202 (Bank Transfers).
- **camt.053**: Bank Statement (replaces MT940).

> **Dev Tip**: ISO 20022 migration is the #1 project in most banks right now.

---

## 2. FIX (Financial Information eXchange)
The standard for **Front-Office Trading**. Optimized for speed (low latency).

### Structure
Tag-Value pairs separated by a delimiter (SOH).
`8=FIX.4.4|9=122|35=D|49=SENDER|56=TARGET|...`

### Key Tags
- **35 (MsgType)**: Defines what the message is.
  - `D`: New Order Single
  - `8`: Execution Report
  - `0`: Heartbeat
  - `A`: Logon
- **OrdStatus (39)**: 0=New, 1=PartiallyFilled, 2=Filled.
- **Side (54)**: 1=Buy, 2=Sell.

### Use Case
- Connecting to Stock Exchanges (CME, LSE).
- Streaming live price quotes.

---

## 3. FpML (Financial Products Markup Language)
The standard for **Derivatives Complexity**. Heavy XML format.

### Use Case
Describes complex OTC products that don't fit in a simple FIX tag.
- Used for: Interest Rate Swaps, CDS, complex options.
- Transported via: Web Services, AMQP (RabbitMQ).

### Snippet Example (IRS)
```xml
<product type="InterestRateSwap">
  <swapStream>
    <payerPartyReference href="party1"/>
    <calculationPeriodAmount>
      <calculation>
        <notionalSchedule>
           <notionalStepSchedule>
             <initialValue>100000000</initialValue>
             <currency>USD</currency>
           </notionalStepSchedule>
        </notionalSchedule>
        <fixedRateSchedule>
          <initialValue>0.05</initialValue>
        </fixedRateSchedule>
      </calculation>
    </calculationPeriodAmount>
  </swapStream>
</product>
```

---

## 4. Comparison Table

| Protocol | Primary Domain | Format | Priority |
|----------|----------------|--------|----------|
| **FIX** | Trading (Front Office) | Tag=Value | Speed (Latency) |
| **SWIFT MT** | Settlement/Payments (Back Office) | Block Text | Reliability |
| **ISO 20022** | Modern Payments | XML | Data Richness |
| **FpML** | Derivatives Description | XML | Complexity |
