# Multi-Exchange Cryptocurrency Arbitrage Analysis

**Python · Pandas · Plotly · Public REST APIs**

A data analytics project that collects real-time cryptocurrency prices from three major exchanges, reconciles them into a unified dataset, and systematically identifies cross-exchange arbitrage opportunities — factoring in round-trip trading fees for realistic net profit estimates.

---

## Business Context

Cryptocurrency markets operate 24/7 across hundreds of global venues with no central clearing mechanism. This fragmentation creates persistent — though often narrow — price discrepancies between exchanges. This project answers a practical question:

> *After accounting for trading fees, do actionable arbitrage opportunities exist between Coinbase, Binance, and Kraken?*

---

## What This Project Demonstrates

| Skill | How it appears in this project |
|---|---|
| **API Integration** | Live data fetched from three public REST endpoints with error handling and rate-limit awareness |
| **Data Reconciliation** | Heterogeneous API responses normalized into a single canonical schema |
| **Financial Analytics** | Gross/net spread calculation, fee modeling, opportunity ranking |
| **Data Visualization** | Four interactive Plotly charts including a multi-panel dashboard |
| **Software Design** | Modular fetcher functions, graceful fallback to sample data, clean separation of concerns |

---

## Exchanges & Assets

| Exchange | API Endpoint Used | Bid/Ask Source |
|---|---|---|
| **Coinbase** | `GET /v2/prices/{pair}/buy` & `/sell` | Indicative fill price |
| **Binance** | `GET /api/v3/ticker/bookTicker` | Level-1 order book |
| **Kraken** | `GET /0/public/Ticker` | Level-1 order book |

**Assets monitored:** BTC · ETH · SOL (all vs. USD)

All endpoints are public — no API keys required.

---

## Analytical Workflow

```
1. Collect   →  Fetch real-time bid/ask prices from all three exchanges
2. Reconcile →  Normalize responses into a unified DataFrame
3. Analyze   →  Compute cross-exchange spreads and net profit after fees
4. Visualize →  Generate interactive charts and save to sample_output/
5. Interpret →  Surface actionable opportunities with business context
```

**Core spread formula:**

```
gross_spread  =  best_bid(Exchange A)  −  best_ask(Exchange B)
net_spread    =  gross_spread  −  (buy_ask × round_trip_fee_rate)
```

An opportunity is flagged as **actionable** when `net_spread > 0` and the buy/sell venues differ.

---

## Project Structure

```
├── crypto_arbitrage_analysis.ipynb   # Main analysis notebook (22 cells)
├── requirements.txt                  # Python dependencies
├── .gitignore
└── sample_output/                    # Auto-generated interactive charts
    ├── price_comparison.html
    ├── spread_heatmap.html
    ├── arbitrage_spread.html
    └── arbitrage_dashboard.html
```

---

## Getting Started

**1. Clone the repository**
```bash
git clone https://github.com/SCastilloP14/Multi-Exchange-Crypto-Arbitrage-Analysis.git
cd Multi-Exchange-Crypto-Arbitrage-Analysis
```

**2. Create and activate a virtual environment**
```bash
python -m venv venv
# Windows
venv\Scripts\activate
# macOS / Linux
source venv/bin/activate
```

**3. Install dependencies**
```bash
pip install -r requirements.txt
```

**4. Launch the notebook**
```bash
jupyter notebook crypto_arbitrage_analysis.ipynb
```

> **Offline / demo mode:** If fewer than two exchanges respond successfully, the notebook automatically falls back to realistic sample data so all cells still execute and all charts still render.

---

## Key Findings

- **Binance** consistently shows the tightest bid-ask spreads across all assets, reflecting its dominant global volume and deep order books.
- **Coinbase** carries a measurable retail premium — its indicative fill prices are slightly wider than institutional venues, which can create a natural sell-side arbitrage leg.
- **Net spreads after a 0.20% round-trip fee are marginal** (0–5 basis points), consistent with well-arbitraged markets. Opportunities exist but require low-fee VIP tiers or co-location infrastructure to capture reliably.
- **SOL** shows the widest percentage spreads of the three assets despite its lower unit price, suggesting comparatively thinner order books.

---

## Visualizations

| Chart | Description |
|---|---|
| **Price Comparison** | Mid prices side-by-side per exchange for each asset |
| **Spread Heatmap** | Bid-ask spread (%) across all exchange × asset combinations |
| **Gross vs Net Spread** | Bar chart contrasting pre-fee and post-fee spreads with break-even line |
| **Arbitrage Dashboard** | 2×2 summary panel covering deviation, best prices, spreads, and net profit |

All charts are interactive (hover, zoom, filter) and exported as standalone HTML files.

---

## Potential Extensions

- **WebSocket feeds** — reduce data latency from ~500 ms (REST polling) to ~10 ms
- **Order book depth** — use size-weighted average price (VWAP) for realistic large-order fill estimates
- **Historical snapshots** — persist data over time to measure spread persistence and mean-reversion
- **Automated alerting** — trigger Slack or email notifications when net spread exceeds a configurable threshold
- **Triangular arbitrage** — extend to multi-leg strategies within a single exchange (e.g. BTC → ETH → SOL → BTC)

---

## Tech Stack

| Library | Purpose |
|---|---|
| `pandas` | Data ingestion, transformation, and reconciliation |
| `numpy` | Numerical calculations |
| `requests` | HTTP calls to exchange REST APIs |
| `plotly` | Interactive visualizations |
| `jupyter` | Notebook environment |

---

*Built as a portfolio project demonstrating applied data analytics in financial markets.*
