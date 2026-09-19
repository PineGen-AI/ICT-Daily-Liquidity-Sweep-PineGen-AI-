# ICT Daily Liquidity Sweep Strategy (Pine Script v6)

An algorithmic, non-repainting **Pine Script v6** strategy for TradingView that automates Inner Circle Trader (ICT) concepts around **Asian Session Liquidity Sweeps** and **Previous Day High/Low (PDH/PDL)** liquidity grabs[cite: 1]. Developed using **[PineGen AI](https://www.pinegen.ai/)**[cite: 1].

---

## Technical Overview

The **ICT Daily Liquidity Sweep Strategy** systematic execution model trades liquidity purges[cite: 1]. It marks the initial price range established during the Asian trading session, monitors price action for stop-hunts beyond key liquidity pools (Asian High/Low or Previous Day High/Low), and enters on confirmed structural rejections back inside the range[cite: 1]. 

To eliminate false breakouts during low-volume periods, execution is strictly restricted to designated session trading windows (e.g., London and New York open hours)[cite: 1].

| Asian Session Range | Liquidity Sweep | Reversal Confirmation | Entry & Exits |
| --- | --- | --- | --- |
| **Window:** 00:00–08:00 UTC | **Trigger:** PDH/PDL or Session High/Low | **Condition:** Close back inside range | **Execution:** Market Order |
| Lock Asian High & Low boundaries | Price trades beyond daily liquidity pools | Bar closes back inside established bounds | **SL:** Sweep Wick + Pad**TP:** Split TP1 / TP2 |

### 1. Asian Session Range Lock
* Tracks the session High and Low during a user-defined period (default: `00:00–08:00 UTC`)[cite: 1].
* The range locks once the session ends, establishing upper and lower liquidity boundaries for the remainder of the day[cite: 1].

### 2. Sweep & Reversal Confirmation
* **Long Setup:** Price trades below the **Asian Low** or **Previous Day's Low (PDL)** and closes back *above* the swept level within the execution window[cite: 1].
* **Short Setup:** Price trades above the **Asian High** or **Previous Day's High (PDH)** and closes back *below* the swept level within the execution window[cite: 1].

### 3. Risk Management & Exits
* **Stop Loss (SL):** Positioned beyond the extreme sweep wick, plus a configurable pip buffer[cite: 1].
* **Dual Take-Profit (TP):**
  * **TP1:** Partial exit at a user-defined R:R multiple (e.g., 1.5R) to secure bankable profit[cite: 1].
  * **TP2:** Full position exit at a secondary R:R multiple (e.g., 3.0R) to capture extended trend moves[cite: 1].
* **Time-Based Hard Exit:** Automatically closes open positions at a specified daily hour to prevent holding through low-liquidity or overnight periods[cite: 1].
* **Dynamic Position Sizing:** Position size is calculated dynamically from account equity and a risk percentage per trade rather than static contract sizes[cite: 1].

---

## Strategy Inputs & Configurable Settings

| Input Category | Variable Name | Default Value | Description |
| :--- | :--- | :--- | :--- |
| **Risk Management** | `SL Buffer (pips)` | Variable | Distance beyond the sweep wick for stop loss placement[cite: 1]. |
| | `Risk %` | `1.0%` | Percentage of total account equity risked per trade[cite: 1]. |
| **Take Profit** | `TP1 / TP2 R:R` | `1.5 / 3.0` | Risk-to-reward multiples for initial and final targets[cite: 1]. |
| | `TP1 Close %` | `50%` | Percentage of position size closed upon reaching TP1[cite: 1]. |
| **Session Control** | `Window Start (UTC)`| `08:00` | Start hour for the allowed execution window[cite: 1]. |
| | `Window End (UTC)` | `17:00` | End hour for the allowed execution window[cite: 1]. |
| **Visuals** | `Labels / Session Backgrounds`| Enabled | Visual toggles for liquidity lines and session boxes[cite: 1]. |

---

## Backtest Assumptions & Execution Model

* **Non-Repainting Execution:** Orders process strictly on `barstate.isconfirmed` at the bar close[cite: 1]. Higher-timeframe data is pulled with lookahead disabled[cite: 1].
* **Starting Equity:** $10,000 baseline[cite: 1].
* **Slippage & Commission:** Modeled with **0.01% commission per side** and **2 ticks of slippage** per fill to reflect real-world execution friction[cite: 1].

---

## Disclaimer

*This strategy is provided for **educational and research purposes only** and does not constitute financial or investment advice[cite: 1]. Backtest results are hypothetical and do not account for live market factors such as execution latency, slippage spikes, or spread widening[cite: 1]. Always forward-test on a demo account before considering live deployment[cite: 1].*
