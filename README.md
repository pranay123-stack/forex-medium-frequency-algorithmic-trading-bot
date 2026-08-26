> ### ⚠️ Design specification — not implemented
>
> **This repository contains a design document only. There is no source code here.**
>
> Everything below describes an intended architecture. Any performance figure, benchmark,
> latency target, throughput number or Sharpe ratio in this document is a **design target
> that has never been measured**, not a result. Installation and usage instructions
> describe files that do not exist in this repository.
>
> It is published as a specification and planning artefact. For systems that are actually
> built and tested, see
> **[crypto-trading-strategies](https://github.com/pranay123-stack/crypto-trading-strategies)**.

---

# Forex Medium Frequency Algorithmic Trading Bot

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat&logo=python&logoColor=white)](https://python.org)
[![C++](https://img.shields.io/badge/C++-17-00599C?style=flat&logo=cplusplus&logoColor=white)](https://isocpp.org/)
[![MT5](https://img.shields.io/badge/MetaTrader-5-FFD700?style=flat)](https://www.metatrader5.com/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

A production-grade medium-frequency algorithmic trading system for forex markets, operating on timeframes from seconds to minutes across major currency pairs.

---

## Overview

| Metric | Value |
|--------|-------|
| **Timeframe** | 1s - 15m |
| **Markets** | Major, Minor, Exotic pairs |
| **Brokers** | OANDA, Interactive Brokers, MT5 |
| **Strategies** | Mean Reversion, Momentum, Carry, News |

---

## Features

- **Multi-Broker Support** - OANDA, Interactive Brokers, MetaTrader 5, cTrader
- **Strategy Framework** - Modular architecture for rapid strategy development
- **Backtesting Engine** - Tick-level replay with realistic spread and slippage
- **Risk Management** - Position sizing, stop-loss, take-profit, max drawdown
- **Economic Calendar** - News event detection and trading filters
- **Session Awareness** - London, New York, Tokyo, Sydney session handling
- **Correlation Analysis** - Cross-pair correlation monitoring

---

## Strategies

| Strategy | Timeframe | Description | Pairs |
|----------|-----------|-------------|-------|
| **Mean Reversion** | 1m - 15m | Statistical arbitrage on RSI/BB extremes | EUR/USD, GBP/USD |
| **Momentum** | 5m - 1h | Trend following with ADX confirmation | All majors |
| **Carry Trade** | 1h - 1D | Interest rate differential capture | AUD/JPY, NZD/JPY |
| **News Trading** | Tick - 1m | High-impact news event scalping | USD pairs |
| **Range Trading** | 5m - 1h | Support/resistance bounce trading | EUR/CHF, USD/CAD |
| **Breakout** | 15m - 4h | Session breakout with volume confirmation | GBP pairs |

---

## Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    FOREX TRADING SYSTEM ARCHITECTURE                     │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌───────────────────────────────────────────────────────────────────┐ │
│  │                        DATA LAYER                                  │ │
│  │  ┌───────────┐  ┌───────────┐  ┌───────────┐  ┌───────────────┐  │ │
│  │  │   OANDA   │  │    IBKR   │  │    MT5    │  │   Economic    │  │ │
│  │  │  Stream   │  │   TWS     │  │  Terminal │  │   Calendar    │  │ │
│  │  └─────┬─────┘  └─────┬─────┘  └─────┬─────┘  └───────┬───────┘  │ │
│  │        └──────────────┴───────┬──────┴────────────────┘          │ │
│  └───────────────────────────────┼──────────────────────────────────┘ │
│                                  ▼                                     │
│  ┌───────────────────────────────────────────────────────────────────┐ │
│  │                    PROCESSING LAYER                                │ │
│  │                                                                    │ │
│  │  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐        │ │
│  │  │   Market     │    │   Signal     │    │    Risk      │        │ │
│  │  │   Data       │───▶│   Generator  │───▶│   Manager    │        │ │
│  │  │   Handler    │    │              │    │              │        │ │
│  │  └──────────────┘    └──────────────┘    └──────┬───────┘        │ │
│  │                                                  │                │ │
│  │  ┌────────────────────────────────────────────────────────────┐  │ │
│  │  │                  STRATEGY ENGINE                            │  │ │
│  │  │  ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌─────────┐ │  │ │
│  │  │  │   Mean     │ │  Momentum  │ │   Carry    │ │  News   │ │  │ │
│  │  │  │ Reversion  │ │            │ │   Trade    │ │ Trading │ │  │ │
│  │  │  └────────────┘ └────────────┘ └────────────┘ └─────────┘ │  │ │
│  │  └────────────────────────────────────────────────────────────┘  │ │
│  └───────────────────────────────┬──────────────────────────────────┘ │
│                                  ▼                                     │
│  ┌───────────────────────────────────────────────────────────────────┐ │
│  │                    EXECUTION LAYER                                 │ │
│  │  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐        │ │
│  │  │    Order     │    │   Position   │    │  Performance │        │ │
│  │  │   Manager    │    │   Tracker    │    │   Analytics  │        │ │
│  │  └──────────────┘    └──────────────┘    └──────────────┘        │ │
│  └───────────────────────────────────────────────────────────────────┘ │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Project Structure

```
forex-medium-frequency-algorithmic-trading-bot/
│
├── src/
│   ├── strategies/
│   │   ├── base_strategy.py
│   │   ├── mean_reversion.py
│   │   ├── momentum.py
│   │   ├── carry_trade.py
│   │   └── news_trading.py
│   │
│   ├── broker/
│   │   ├── oanda.py
│   │   ├── ibkr.py
│   │   └── mt5.py
│   │
│   ├── data/
│   │   ├── feed.py
│   │   ├── historical.py
│   │   └── calendar.py
│   │
│   ├── risk/
│   │   ├── position_sizer.py
│   │   └── risk_manager.py
│   │
│   ├── backtest/
│   │   ├── engine.py
│   │   └── metrics.py
│   │
│   └── utils/
│       ├── config.py
│       ├── logger.py
│       └── indicators.py
│
├── config/
│   └── config.yaml
│
├── tests/
├── notebooks/
├── requirements.txt
└── README.md
```

---

## Quick Start

```bash
# Clone repository
git clone https://github.com/pranay123-stack/forex-medium-frequency-algorithmic-trading-bot.git
cd forex-medium-frequency-algorithmic-trading-bot

# Install dependencies
pip install -r requirements.txt

# Configure
cp config/config.example.yaml config/config.yaml

# Run backtest
python -m src.backtest.engine --strategy mean_reversion --pair EUR/USD

# Run live (paper)
python -m src.main --strategy momentum --mode paper
```

---

## Configuration

```yaml
# config/config.yaml
broker:
  name: oanda
  account_type: practice  # practice or live
  api_key: ${OANDA_API_KEY}

trading:
  pairs:
    - EUR/USD
    - GBP/USD
    - USD/JPY
  timeframe: 5m
  max_positions: 3

risk:
  max_position_size: 0.02   # 2% risk per trade
  stop_loss_pips: 20
  take_profit_pips: 40
  max_daily_loss: 0.05      # 5% max daily loss
```

---

## Coming Soon

- [ ] Strategy implementations
- [ ] Broker connectors (OANDA, IBKR, MT5)
- [ ] Backtesting engine
- [ ] Economic calendar integration
- [ ] Web dashboard

---

## Risk Warning

**Forex trading involves substantial risk of loss.** Leverage can work against you. Past performance does not guarantee future results. Only trade with capital you can afford to lose.

---

## License

MIT License

---

## Contact

**Pranay** - Quantitative Developer

[![GitHub](https://img.shields.io/badge/GitHub-pranay123--stack-181717?style=flat&logo=github)](https://github.com/pranay123-stack)
