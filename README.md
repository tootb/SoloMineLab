# SoloMineLab — Bitaxe & NerdQaxe Native Stratum V2 Solo Mining Guide

Official open-source setup guide for connecting **Bitaxe (AxeOS v2.14.0+)**, **NerdQaxe (NerdOS v1.0.37+)**, and standard SHA-256 / Scrypt ASICs to **[SoloMineLab](https://solominelab.com/)**.

* **Main Website:** [https://solominelab.com/](https://solominelab.com/)
* **Unified Mining Guide:** [https://solominelab.com/mining-guide](https://solominelab.com/mining-guide)
* **Fee Model:** **0.2% fee forever** — 100% non-custodial direct coinbase payouts (`99.8%` to miner in `vout[0]` / `0.2%` pool fee in `vout[1]`, `0%` on shares, no account required)
* **Live Telemetry & Alerts:** Free Telegram bot [`@btcminerpool_bot`](https://t.me/btcminerpool_bot)
* **Official X:** [`@tootbcrypto`](https://x.com/tootbcrypto)

---

## Live Stratum Endpoints

| Coin / Mode | Protocol | Endpoint | Username Format | Password | Fee |
| :--- | :--- | :--- | :--- | :--- | :---: |
| **Bitcoin (BTC) Solo** | **Native Stratum V2 (Extended Channels)** | `btc.solominelab.com:3338` | `YOUR_BTC_WALLET.bitaxe1` | `x` | **0.2%** |
| **Bitcoin (BTC) Solo** | Stratum V1 (TCP) | `stratum+tcp://btc.solominelab.com:3333` | `YOUR_BTC_WALLET.bitaxe1` | `x` | **0.2%** |
| **Bitcoin Cash (BCH) Solo** | **Native Stratum V2 (Extended Channels)** | `bch.solominelab.com:3339` | `bitcoincash:YOUR_CASHADDR.bitaxe1` | `x` | **0.2%** |
| **Bitcoin Cash (BCH) Solo** | Stratum V1 (TCP) | `stratum+tcp://bch.solominelab.com:3334` | `bitcoincash:YOUR_CASHADDR.bitaxe1` | `x` | **0.2%** |
| **Litecoin (LTC) Solo** | Stratum V1 (TCP) | `stratum+tcp://ltc.solominelab.com:3335` | `YOUR_LTC_WALLET.worker1` | `x` | **0.2%** |
| **Merged LTC + DOGE** | Stratum V1 (Dual-Coinbase) | `stratum+tcp://ltc.solominelab.com:3336` | `YOUR_LTC_WALLET.worker1` | `doge=YOUR_DOGE_WALLET` | **0.2%** |
| **Dogecoin (DOGE) Solo** | Stratum V1 (TCP, no LTC wallet) | `stratum+tcp://doge.solominelab.com:3337` | `YOUR_DOGE_WALLET.worker1` | `x` | **0.2%** |

### Native Stratum V2 Authority Public Keys (Noise NX)

* **Bitcoin (`btc.solominelab.com:3338`):**
  ```text
  9bnBU6uuuXam8hGfkkkiAFH763q3AEKmBZy2vihPekjM5Md4hLR
