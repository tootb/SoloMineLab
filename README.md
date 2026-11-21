# SoloMineLab - Bitaxe & NerdQaxe Native Stratum V2 Solo Mining Guide

Official public setup guide for connecting **Bitaxe (AxeOS v2.14.0+)**, **NerdQaxe (NerdOS v1.0.37+)**, and standard SHA-256 / Scrypt ASICs to **[SoloMineLab](https://solominelab.com/)**.

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

Each coin uses its own independent SV2 Authority Public Key in production:

* **BTC Solo (`btc.solominelab.com:3338`):** `9bnBU6uuuXam8hGfkkkiAFH763q3AEKmBZy2vihPekjM5Md4hLR`
* **BCH Solo (`bch.solominelab.com:3339`):** `9b2X5AM1xH8pLER84gTtHsA216LmkgfgQA1GyXENcMxujRJC8XZ`

> Always verify live port status and current keys at [https://solominelab.com/docs?protocol=v2](https://solominelab.com/docs?protocol=v2) and [https://solominelab.com/mining-guide](https://solominelab.com/mining-guide).

---

## 1. Bitaxe Setup — AxeOS v2.14.0+ (Native Stratum V2)

No `stratum2+tcp://` prefix or local proxy is needed on Bitaxe/NerdQaxe.

1. Open your Bitaxe dashboard (`http://<BITAXE_LOCAL_IP>`) and go to **Settings → Pool Settings**.
2. Set Protocol to **Stratum V2 + Extended Channels** and configure your **Primary Pool**:
   * **BTC Solo:** Host `btc.solominelab.com` | Port `3338` | SV2 Authority Pubkey `9bnBU6uuuXam8hGfkkkiAFH763q3AEKmBZy2vihPekjM5Md4hLR` | User `bc1qYOUR_BTC_WALLET.bitaxe1` | Pass `x`
   * **BCH Solo:** Host `bch.solominelab.com` | Port `3339` | SV2 Authority Pubkey `9b2X5AM1xH8pLER84gTtHsA216LmkgfgQA1GyXENcMxujRJC8XZ` | User `bitcoincash:YOUR_CASHADDR.bitaxe1` *(include `bitcoincash:`)* | Pass `x`
3. *(Recommended)* Set your **Fallback Pool** to Stratum V1 (`btc.solominelab.com:3333` for BTC or `bch.solominelab.com:3334` for BCH) using the same wallet username and password `x`.
4. Click **Save** and **Restart**.

---

## 2. NerdQaxe Setup — NerdOS v1.0.37+ (Native Stratum V2)

1. Open NerdOS (`http://<NERDQAXE_LOCAL_IP>`) and navigate to **Settings → Pool Configuration**.
2. Select **Stratum V2 + Extended Channels** and enter:
   * **BTC Solo:** Host `btc.solominelab.com` | Port `3338` | Auth Key `9bnBU6uuuXam8hGfkkkiAFH763q3AEKmBZy2vihPekjM5Md4hLR` | User `bc1qYOUR_BTC_WALLET.nerdqaxe1` | Pass `x`
   * **BCH Solo:** Host `bch.solominelab.com` | Port `3339` | Auth Key `9b2X5AM1xH8pLER84gTtHsA216LmkgfgQA1GyXENcMxujRJC8XZ` | User `bitcoincash:YOUR_CASHADDR.nerdqaxe1` | Pass `x`
3. *(Recommended)* Configure Stratum V1 (`:3333` for BTC / `:3334` for BCH) as your **Fallback Pool**.
4. Save and reboot. Confirm in the live log that the Noise NX handshake completes and shares are accepted.

---

## 3. Verifying Non-Custodial Payouts On-Chain (99.8% / 0.2%)

SoloMineLab never holds miner funds. Every block template writes the payout directly into the block's coinbase transaction (`tx[0]`):

* **`vout[0]` → 99.8%** of the block subsidy + transaction fees paid directly to the winning miner's address.
* **`vout[1]` → 0.2%** permanent pool fee.

For Scrypt merged mining on port `:3336`, **both** Litecoin and Dogecoin generate independent non-custodial coinbase payouts directly to your respective LTC and DOGE wallets.

Full step-by-step non-custodial payout details: **[https://solominelab.com/mining-guide#non-custodial](https://solominelab.com/mining-guide#non-custodial)**

---

## 4. Real-Time Dashboards & Telegram Telemetry

No login or account needed — track hashrate, Best Share, VarDiff, and worker status directly with your payout address:

* **BTC Pool:** [https://btc.solominelab.com/](https://btc.solominelab.com/)
* **BCH Pool:** [https://bch.solominelab.com/](https://bch.solominelab.com/)
* **LTC & Merged Pool:** [https://ltc.solominelab.com/](https://ltc.solominelab.com/)
* **DOGE Solo Pool:** [https://doge.solominelab.com/](https://doge.solominelab.com/)
* **Telegram Bot:** [`@btcminerpool_bot`](https://t.me/btcminerpool_bot) — instant offline alerts, Best Share notifications, and live worker stats.

---

## Links

* Website: [https://solominelab.com/](https://solominelab.com/)
* Mining Guide: [https://solominelab.com/mining-guide](https://solominelab.com/mining-guide)
* X / Twitter: [`@tootbcrypto`](https://x.com/tootbcrypto)
