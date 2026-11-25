# SoloMineLab — Non-Custodial Solo & Merged Mining Pool (0.2% Fee Forever)

[![Website](https://img.shields.io/badge/Website-solominelab.com-f7931a?style=flat-square)](https://solominelab.com/)
[![Pool Fee](https://img.shields.io/badge/Block%20Fee-0.2%25%20Forever-10b981?style=flat-square)](https://solominelab.com/solo-mining-pool-comparison)
[![Payout Model](https://img.shields.io/badge/Payout-99.8%25%20Direct%20Coinbase-2563eb?style=flat-square)](https://solominelab.com/non-custodial-solo-mining)
[![Stratum V2](https://img.shields.io/badge/Native%20Stratum%20V2-BTC%20%3A3338%20%7C%20BCH%20%3A3339-7c3aed?style=flat-square)](https://solominelab.com/stratum-v2-bitcoin-guide)
[![Merged Solo](https://img.shields.io/badge/LTC%20%2B%20DOGE-Merged%20Solo%20%3A3336-eab308?style=flat-square)](https://solominelab.com/ltc-doge-merged-solo-mining)
[![Telegram](https://img.shields.io/badge/Telegram-%40solominelab-26A5E4?style=flat-square)](https://t.me/solominelab)
[![License: MIT](https://img.shields.io/badge/License-MIT-64748b?style=flat-square)](LICENSE)

> **SoloMineLab** is a **100% non-custodial** cryptocurrency solo and AuxPoW merged mining pool infrastructure for **Bitcoin (BTC)**, **Bitcoin Cash (BCH)**, **Litecoin (LTC)**, and **Dogecoin (DOGE)**. It charges a permanent **0.2% block-only operator fee** (**10× lower than Solo CKPool's 2.0%**), pays **99.8%** directly to the winning miner's self-custodial wallet in coinbase `vout[0]` (`0%` fee on shares, zero accounts, zero pool wallet custody), operates **native encrypted Noise NX Stratum V2** on both **BTC (`:3338`)** and **BCH (`:3339`)**, and runs **dual-coinbase LTC+DOGE merged solo mining (`:3336`)** on co-located mainnet full nodes with local ZMQ `hashblock` push notifications.

This repository hosts SoloMineLab's official public connection reference, hardware quick-start guides, and [`verify_solominelab_coinbase.py`](verify_solominelab_coinbase.py) — a **zero-dependency Python 3 script** that lets any miner cryptographically verify our `/SoloMineLabCom/` coinbase tag and **99.8% / 0.2%** direct payout split **live before a block is ever found**.

---

## Contents

- [Why SoloMineLab? (2026 Solo Pool Comparison)](#why-solominelab-2026-solo-pool-comparison)
- [Live Mainnet Stratum Endpoints & Difficulty Tiers](#live-mainnet-stratum-endpoints--difficulty-tiers)
- [Native Stratum V2 Authority Public Keys (Noise NX)](#native-stratum-v2-authority-public-keys-noise-nx)
- [Pre-Block Cryptographic Coinbase Verification (`verify_solominelab_coinbase.py`)](#pre-block-cryptographic-coinbase-verification-verify_solominelab_coinbasepy)
- [Hardware Compatibility & Quick Setup Guides](#hardware-compatibility--quick-setup-guides)
- [Non-Custodial Architecture & Security Guarantees](#non-custodial-architecture--security-guarantees)
- [Official Documentation, Calculators & Guides](#official-documentation-calculators--guides)
- [Live Dashboards, Telegram Telemetry & Community](#live-dashboards-telegram-telemetry--community)

---

## Why SoloMineLab? (2026 Solo Pool Comparison)

| Solo Pool | Block Fee | Payout & Custody Model | Supported Coins | Native Stratum V2 | LTC+DOGE Merged Solo | Account Required |
| :--- | :---: | :--- | :--- | :--- | :--- | :---: |
| **[SoloMineLab](https://solominelab.com/)** | **0.2% forever** *(0% shares)* | **Non-custodial direct coinbase** (`vout[0]` 99.8%, `/SoloMineLabCom/`) | **BTC, BCH, LTC, DOGE** | **Yes — BTC (`:3338`) & BCH (`:3339`)** | **Yes (`:3336`, dual direct coinbase, 0.2%)** | **No** |
| Solo CKPool (`solo.ckpool.org`) | 2.0% | Direct coinbase (98%) | BTC only | Yes (BTC `:3336` only) | No | No |
| Public Pool (`public-pool.io` / self-host) | 0% | Direct coinbase (100%) | BTC (primarily) | Yes (BTC) | No | No |
| BCMonster | 1.5% | Direct solo port | BTC, BCH, others | Selected ports | No | No |
| ViaBTC Solo | 1.0% | Custodial account ledger | BTC, BCH, LTC, others | No | Custodial PPS+/PPLNS (2–4%) | Yes |
| KanoPool | 0.9% | Pool accounting | BTC only | No | No | Yes |
| MySoloPool | ~20% LTC / 0% DOGE | Solo pool accounting | LTC + DOGE | No | Yes (high LTC fee) | No |
| VEIN Pool | 1.0% | Solo pool | LTC solo | No | No | No |

**Concrete Bitcoin Block Savings (3.125 BTC subsidy, excluding tx fees):**
- On a **2.0%** solo pool (e.g., Solo CKPool), the pool deducts **0.0625 BTC** from a found block.
- On **SoloMineLab (0.2%)**, the pool deducts **0.00625 BTC** — saving the winning miner **0.05625 BTC (10× lower fee)** on every block while keeping zero custody, native Noise NX Stratum V2 encryption, and 4 co-located full nodes.

Full fact-checked comparisons: **[Solo Mining Pool Comparison (2026)](https://solominelab.com/solo-mining-pool-comparison)** · **[SoloMineLab vs Solo CKPool](https://solominelab.com/solominelab-vs-ckpool)**

---

## Live Mainnet Stratum Endpoints & Difficulty Tiers

All 17 production ports enforce **fail-closed mainnet wallet validation**, **0.2% permanent block-only fee**, and **0% fee on shares**:

| Coin / Mode | Algorithm | Protocol | Host | Auto VarDiff | Mid-Diff | High-Diff | Username Format | Password |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- |
| **Bitcoin (BTC) Solo** | SHA-256d | Stratum V1 (TCP) | `btc.solominelab.com` | `:3333` *(start 1,024 / min 64)* | `:4333` *(start 65,536 / min 32,768)* | `:5333` *(floor 1,000,000)* | `bc1qYOUR_BTC_WALLET.worker` | `x` *(or `d=DIFF`)* |
| **Bitcoin (BTC) Solo** | SHA-256d | **Native Stratum V2** | `btc.solominelab.com` | **`:3338`** *(instant auto-diff)* | — | — | `bc1qYOUR_BTC_WALLET.worker` | `x` |
| **Bitcoin Cash (BCH) Solo** | SHA-256d | Stratum V1 (TCP) | `bch.solominelab.com` | `:3334` *(start 1,024 / min 64)* | `:4334` *(start 65,536 / min 32,768)* | `:5334` *(floor 1,000,000)* | `bitcoincash:qYOUR_WALLET.worker` | `x` *(or `d=DIFF`)* |
| **Bitcoin Cash (BCH) Solo** | SHA-256d | **Native Stratum V2** | `bch.solominelab.com` | **`:3339`** *(instant auto-diff)* | — | — | `bitcoincash:qYOUR_WALLET.worker` | `x` |
| **Litecoin (LTC) Solo** | Scrypt | Stratum V1 (TCP) | `ltc.solominelab.com` | `:3335` *(start 4,096 / min 64)* | `:4335` *(start 131,072 / min 65,536)* | `:5335` *(floor 4,194,304)* | `ltc1qYOUR_LTC_WALLET.worker` | `x` *(or `d=DIFF`)* |
| **LTC + DOGE Merged Solo** | Scrypt AuxPoW | **Dual-Coinbase V1** | `ltc.solominelab.com` | **`:3336`** *(start 4,096 / min 64)* | `:4336` *(start 131,072 / min 65,536)* | `:5336` *(floor 4,194,304)* | `YOUR_LTC_WALLET.worker` | `doge=YOUR_DOGE_WALLET` |
| **Dogecoin (DOGE) Solo** | Scrypt AuxPoW | Stratum V1 *(no LTC)* | `doge.solominelab.com` | `:3337` *(start 4,096 / min 64)* | `:4337` *(start 131,072 / min 65,536)* | `:5337` *(floor 4,194,304)* | `DYOUR_DOGE_WALLET.worker` | `x` *(or `d=DIFF`)* |

> **Merged Mining Tip (`:3336` / `:4336` / `:5336`):** If your ASIC firmware restricts `=` in the password field, use dual-username syntax instead: Username `YOUR_LTC_WALLET+YOUR_DOGE_WALLET.worker` with Password `x`.  
> **Scrypt Difficulty Scale:** Stratum difficulty `4,194,304` corresponds to chain difficulty `64` (`Stratum Diff ÷ 65,536`).  
> **Interactive Builder:** Generate copy-ready settings for your exact rig at **[https://solominelab.com/docs](https://solominelab.com/docs)**.

---

## Native Stratum V2 Authority Public Keys (Noise NX)

Both Bitcoin and Bitcoin Cash run independent Noise NX encrypted Stratum V2 endpoints (**ElligatorSwift + ChaCha20-Poly1305 AEAD**, **BIP340 Schnorr authority authentication**, **Extended Channels**, and **Job Declaration disabled** so every template enforces the 99.8% / 0.2% non-custodial coinbase split and zero-SegWit CTOR safety on BCH):

- **Bitcoin (BTC — `btc.solominelab.com:3338`)**
  - Authority Public Key: `9bnBU6uuuXam8hGfkkkiAFH763q3AEKmBZy2vihPekjM5Md4hLR`
  - Full SV2 URI *(Braiins OS+ / SRI Proxy)*:
    ```text
    stratum2+tcp://btc.solominelab.com:3338/9bnBU6uuuXam8hGfkkkiAFH763q3AEKmBZy2vihPekjM5Md4hLR
    ```
- **Bitcoin Cash (BCH — `bch.solominelab.com:3339`)**
  - Authority Public Key: `9b2X5AM1xH8pLER84gTtHsA216LmkgfgQA1GyXENcMxujRJC8XZ`
  - Full SV2 URI *(Proxy / CLI)*:
    ```text
    stratum2+tcp://bch.solominelab.com:3339/9b2X5AM1xH8pLER84gTtHsA216LmkgfgQA1GyXENcMxujRJC8XZ
    ```

> Always verify live port status and current keys at **[solominelab.com/stratum-v2-bitcoin-guide](https://solominelab.com/stratum-v2-bitcoin-guide)** and **[solominelab.com/bch-stratum-v2-guide](https://solominelab.com/bch-stratum-v2-guide)**.

---

## Pre-Block Cryptographic Coinbase Verification (`verify_solominelab_coinbase.py`)

**Don't trust — verify.** In Proof-of-Work mining, the coinbase transaction is hashed into the block Merkle root before your ASIC starts hashing. Changing even one byte of an output address invalidates the block hash. Every live SoloMineLab Stratum job contains:

| Coinbase Field | Verified Content |
| :--- | :--- |
| **`vin[0].scriptSig`** | BIP34 block height + ASCII **`/SoloMineLabCom/`** (`2f536f6c6f4d696e654c6162436f6d2f`) + extranonce *(+ AuxPoW `fabe6d6d` on `:3336`/`:3337`)* |
| **`vout[0]` (Miner)** | **99.8%** of total block reward (subsidy + tx fees) locked directly to **your wallet's exact `scriptPubKey`** |
| **`vout[1]` (Operator)** | **0.2%** permanent pool operator fee |
| **`vout[2]` (Witness)** | `0`-value BIP141 SegWit commitment `OP_RETURN aa21a9ed...` on BTC & LTC only *(strictly omitted on pure non-SegWit BCH)* |

### Run the Live Zero-Dependency Python 3 Verifier (10 Seconds)

[`verify_solominelab_coinbase.py`](verify_solominelab_coinbase.py) requires **Python 3.8+ standard library only** (no `pip install`). It connects directly to SoloMineLab, captures one real `mining.notify` job, derives your wallet's `scriptPubKey` locally, verifies `/SoloMineLabCom/` and the **99.8% / 0.2%** split, and prints the raw coinbase hex for `bitcoin-cli decoderawtransaction`:

```bash
# Bitcoin (BTC) Solo — port 3333 (add --check-fail-closed to also test non-wallet rejection)
python3 verify_solominelab_coinbase.py --coin btc --wallet bc1qYOUR_BTC_ADDRESS --check-fail-closed

# Bitcoin Cash (BCH) Solo — port 3334 (verifies CashAddr payout + zero SegWit outputs)
python3 verify_solominelab_coinbase.py --coin bch --wallet bitcoincash:qYOUR_BCH_ADDRESS

# Litecoin (LTC) Solo — port 3335
python3 verify_solominelab_coinbase.py --coin ltc --wallet ltc1qYOUR_LTC_ADDRESS

# LTC + DOGE Merged Solo — port 3336 (verifies LTC 99.8% vout[0] + fabe6d6d AuxPoW commitment)
python3 verify_solominelab_coinbase.py --coin merged --wallet YOUR_LTC_ADDRESS --doge-wallet DYOUR_DOGE_ADDRESS

# Dogecoin (DOGE) Solo AuxPoW — port 3337 (verifies fail-closed DOGE auth + /SoloMineLabCom/ + fabe6d6d)
python3 verify_solominelab_coinbase.py --coin doge --wallet DYOUR_DOGE_ADDRESS

# Optional: inspect Mid/High difficulty ports or output machine-readable JSON
python3 verify_solominelab_coinbase.py --coin btc --port 5333 --wallet bc1qYOUR_BTC_ADDRESS --json
```

**Cross-check with your own full node:**
```bash
bitcoin-cli decoderawtransaction <RAW_COINBASE_HEX>
```

> **Stratum V2 Verification:** On BTC (`:3338`) and BCH (`:3339`), concatenating `coinbase_tx_prefix + extranonce_bytes + coinbase_tx_outputs_suffix` from any decrypted `NewExtendedMiningJob` message produces the exact same `/SoloMineLabCom/` tag, **99.8%** miner `vout[0]`, and **0.2%** operator `vout[1]`. Full walkthrough: **[https://solominelab.com/verify-coinbase](https://solominelab.com/verify-coinbase)**.

---

## Hardware Compatibility & Quick Setup Guides

### Supported Miners & Recommended Ports

| Hardware Category | Models | Typical Hashrate | Recommended SoloMineLab Port | Detailed Guide |
| :--- | :--- | :---: | :--- | :--- |
| **Bitaxe** | Ultra, Supra, Gamma 601/602, Hex, GT | 0.5–3.6 TH/s | BTC `:3338` (SV2) / `:3333` (V1) · BCH `:3339` / `:3334` | [Bitaxe Solo Guide](https://solominelab.com/bitaxe-solo-mining-guide) |
| **NerdQaxe / NerdOctaxe** | Gaia, NerdQaxe++, Hydro, NerdOctaxe Gamma | 2.6–12 TH/s | BTC `:3338` (SV2) / `:3333` (V1) · BCH `:3339` / `:3334` | [NerdQaxe++ Guide](https://solominelab.com/nerdqaxe-solo-mining-guide) |
| **Home SHA-256** | Avalon Nano 3 / 3S, LuckyMiner LV06–LV08, Bitcube | 0.5–6 TH/s | BTC `:3333` (V1) · BCH `:3334` (V1) *(or LAN SV2 proxy)* | [Avalon Nano 3/3S Guide](https://solominelab.com/avalon-nano-3-3s-solo-mining) |
| **Industrial SHA-256** | Antminer S19 / S21, WhatsMiner M50–M60 | 90–270+ TH/s | Mid `:4333`/`:4334` · High `:5333`/`:5334` · SV2 `:3338`/`:3339` | [Stratum V2 Guide](https://solominelab.com/stratum-v2-bitcoin-guide) |
| **Home Scrypt** | Goldshell Mini-DOGE / III, FutureBit Apollo, L3+ | 0.2–3.35 GH/s | Merged `:3336` · LTC `:3335` · DOGE `:3337` | [Scrypt Mining Guide](https://solominelab.com/scrypt-mining-guide) |
| **Mid Scrypt ASIC** | Fluminer L1, ElphaPex DG Home 1, VolcMiner D1 Mini | 2.1–5.3 GH/s | Mid Merged `:4336` · LTC `:4335` · DOGE `:4337` | [LTC+DOGE Merged Guide](https://solominelab.com/ltc-doge-merged-solo-mining) |
| **High Scrypt ASIC** | Antminer L7 / L9, ElphaPex DG1+, VolcMiner D1 / Hydro | 9–30.4+ GH/s | High Merged `:5336` · LTC `:5335` · DOGE `:5337` | [Antminer L9/L7/DG1+ Setup](https://solominelab.com/antminer-l9-l7-solo-mining-setup) |

### 1. Bitaxe (AxeOS v2.14.0+ — Native Stratum V2)

No `stratum2+tcp://` prefix or local proxy is needed on AxeOS v2.14.0+.

1. Open your Bitaxe UI (`http://<BITAXE_LOCAL_IP>`) → **Settings → Pool Settings**.
2. Set Protocol to **Stratum V2 + Extended Channels** and configure your **Primary Pool**:
   - **BTC Solo:** Host `btc.solominelab.com` | Port `3338` | SV2 Authority Pubkey `9bnBU6uuuXam8hGfkkkiAFH763q3AEKmBZy2vihPekjM5Md4hLR` | User `bc1qYOUR_BTC_WALLET.bitaxe1` | Pass `x`
   - **BCH Solo:** Host `bch.solominelab.com` | Port `3339` | SV2 Authority Pubkey `9b2X5AM1xH8pLER84gTtHsA216LmkgfgQA1GyXENcMxujRJC8XZ` | User `bitcoincash:qYOUR_CASHADDR.bitaxe1` *(include `bitcoincash:`)* | Pass `x`
3. *(Recommended)* Configure **Fallback Pool** as Stratum V1 (`btc.solominelab.com:3333` for BTC or `bch.solominelab.com:3334` for BCH) using the same wallet username and password `x`.
4. Click **Save** and **Restart**.

### 2. NerdQaxe++ / NerdOctaxe (NerdOS v1.0.37+ — Native Stratum V2)

1. Open NerdOS (`http://<NERDQAXE_LOCAL_IP>`) → **Settings → Pool Configuration**.
2. Select **Stratum V2 + Extended Channels** *(keep Require Authentication enabled)*:
   - **BTC Solo:** Host `btc.solominelab.com` | Port `3338` | Auth Key `9bnBU6uuuXam8hGfkkkiAFH763q3AEKmBZy2vihPekjM5Md4hLR` | User `bc1qYOUR_BTC_WALLET.nerdqaxe1` | Pass `x`
   - **BCH Solo:** Host `bch.solominelab.com` | Port `3339` | Auth Key `9b2X5AM1xH8pLER84gTtHsA216LmkgfgQA1GyXENcMxujRJC8XZ` | User `bitcoincash:qYOUR_CASHADDR.nerdqaxe1` | Pass `x`
3. *(Recommended)* Set Stratum V1 (`:3333` BTC / `:3334` BCH) as **Fallback Pool**, save, and reboot.

### 3. Scrypt ASICs — Antminer L9/L7, ElphaPex DG1+, VolcMiner D1 (LTC + DOGE Merged Solo)

Every Scrypt hash checks **both Litecoin and Dogecoin block templates simultaneously** with zero hashrate split and two independent direct coinbases (`99.8%` LTC + `99.8%` DOGE):

- **Pool 1 (High-Diff Merged — ≥9 GH/s):** `stratum+tcp://ltc.solominelab.com:5336` *(or `:4336` Mid / `:3336` Auto)*
- **Worker:** `YOUR_LTC_WALLET.l9_rig1`
- **Password:** `doge=YOUR_DOGE_WALLET`
- **Pool 2 (Auto-Diff Merged Failover):** `stratum+tcp://ltc.solominelab.com:3336` with the same worker & password.

> **Self-Custodial Wallet Warning:** Never mine directly to a centralized exchange deposit address — many exchanges fail to credit coinbase block outputs. Always use a self-custodial wallet whose private keys you control (`bc1q`/`bc1p` BTC, `bitcoincash:q...` BCH, `ltc1q`/`L`/`M` LTC, `D...` DOGE). Standard protocol coinbase maturity applies (**100 confirmations** on BTC/BCH/LTC, **240 confirmations** on DOGE).

---

## Non-Custodial Architecture & Security Guarantees

- **4 Co-Located Mainnet Full Nodes:** Dedicated `bitcoind` (BTC), `Bitcoin Cash Node` (BCH), `litecoind` (LTC SegWit+MWEB), and `dogecoind` (DOGE) wired directly to local Stratum engines via **ZMQ `hashblock`** push notifications for millisecond block-template propagation.
- **7 Isolated Stratum Engines:** 5 Stratum V1 / AuxPoW engines + 2 native Noise NX Stratum V2 engines running under least-privilege Linux service isolation.
- **Fail-Closed Wallet Validation:** Plain worker names or malformed/cross-network addresses are rejected immediately at `mining.authorize` so block rewards can never fall back to a pool treasury.
- **Dual-Coinbase AuxPoW (No Custodial `createauxblock`):** Both LTC+DOGE Merged (`:3336`) and DOGE Solo (`:3337`) build Dogecoin templates via `getblocktemplate` with direct miner `vout[0]` outputs, never single-address custodial `createauxblock`.

Deep dive: **[Non-Custodial Solo Mining Architecture](https://solominelab.com/non-custodial-solo-mining)** · **[About SoloMineLab](https://solominelab.com/about)**

---

## Official Documentation, Calculators & Guides

### Core Tools & Benchmarks
- **[Interactive Stratum Configurator & Block Calculator](https://solominelab.com/docs)** — Device presets, SV2 keys, and live probability calculator
- **[Live Solo Mining Odds (BTC, BCH, LTC & DOGE)](https://solominelab.com/solo-mining-odds)** — Real-time Poisson block odds from mainnet full-node difficulty
- **[Solo Mining Pool Comparison (2026)](https://solominelab.com/solo-mining-pool-comparison)** — 8 solo pools compared on fees, custody, SV2, and Scrypt merged
- **[SoloMineLab vs Solo CKPool](https://solominelab.com/solominelab-vs-ckpool)** — 0.2% vs 2.0% breakdown & 60-second switching guide
- **[Bitcoin Solo Hardware Catalog](https://miners.solominelab.com/)** — Live EUR comparison of Bitaxe, NerdQaxe++, NerdOctaxe, Bitcube & Avalon Nano 3S

### Protocol, Verification & Hardware Guides
- **[Complete Solo Mining Setup Guide](https://solominelab.com/mining-guide)**
- **[Verify Non-Custodial Coinbase Payouts](https://solominelab.com/verify-coinbase)**
- **[Non-Custodial Solo Mining Architecture](https://solominelab.com/non-custodial-solo-mining)**
- **[Bitcoin & BCH Native Stratum V2 Guide](https://solominelab.com/stratum-v2-bitcoin-guide)** · **[BCH Stratum V2 (:3339) Guide](https://solominelab.com/bch-stratum-v2-guide)**
- **[Bitaxe Solo Mining Guide](https://solominelab.com/bitaxe-solo-mining-guide)** · **[NerdQaxe++ & NerdOctaxe Guide](https://solominelab.com/nerdqaxe-solo-mining-guide)** · **[Avalon Nano 3 & 3S Guide](https://solominelab.com/avalon-nano-3-3s-solo-mining)**
- **[Bitcoin (BTC) vs Bitcoin Cash (BCH) Solo Mining](https://solominelab.com/bitcoin-vs-bitcoin-cash-solo-mining)**
- **[Scrypt Solo & Merged Guide](https://solominelab.com/scrypt-mining-guide)** · **[LTC+DOGE Merged Solo (:3336)](https://solominelab.com/ltc-doge-merged-solo-mining)** · **[Antminer L9/L7, ElphaPex DG1+ & VolcMiner D1 Setup](https://solominelab.com/antminer-l9-l7-solo-mining-setup)**
- **[Solo Mining & Stratum V2 Glossary](https://solominelab.com/solo-mining-glossary)** · **[About SoloMineLab](https://solominelab.com/about)**

### Machine-Readable & AI Context
- **[`llms.txt`](https://solominelab.com/llms.txt)** · **[`llms-full.txt`](https://solominelab.com/llms-full.txt)** · **[`sitemap-index.xml`](https://solominelab.com/sitemap-index.xml)**

---

## Live Dashboards, Telegram Telemetry & Community

No account or login required — track live hashrate, Best Share, VarDiff, stale rates, and 24H/7D/30D worker charts directly with your payout address:

- **All Pools Overview:** [https://solominelab.com/](https://solominelab.com/)
- **Bitcoin (BTC) Solo Pool:** [https://btc.solominelab.com/](https://btc.solominelab.com/)
- **Bitcoin Cash (BCH) Solo Pool:** [https://bch.solominelab.com/](https://bch.solominelab.com/)
- **Litecoin (LTC) Solo & Merged Pool:** [https://ltc.solominelab.com/](https://ltc.solominelab.com/)
- **Dogecoin (DOGE) Solo Pool:** [https://doge.solominelab.com/](https://doge.solominelab.com/)
- **Telegram Community:** [https://t.me/solominelab](https://t.me/solominelab) — direct operator support and live network updates
- **Free Telegram Wallet Bot:** [`@btcminerpool_bot`](https://t.me/btcminerpool_bot) — private DM worker checks, offline alerts, Best Share & block notifications
- **Official Operator X (Twitter):** [`@tootbcrypto`](https://x.com/tootbcrypto)

---

## License

Released under the [MIT License](LICENSE).
