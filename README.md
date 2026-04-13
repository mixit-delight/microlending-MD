# microlending-MD
LumenCredit — Decentralized Micro-Lending &amp; Credit Scoring on Stellar  Financial access for the unbanked. On-chain loan history as a portable, verifiable credit score.
# LumenCredit — Decentralized Micro-Lending & Credit Scoring on Stellar

> Financial access for the unbanked. On-chain loan history as a portable, verifiable credit score.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Built on Stellar](https://img.shields.io/badge/Built%20on-Stellar-7B2FBE)](https://stellar.org)
[![Soroban](https://img.shields.io/badge/Smart%20Contracts-Soroban-1D9E75)](https://soroban.stellar.org)
[![Stellar Wave](https://img.shields.io/badge/Stellar-Wave%20Program-F5A623)](https://drips.network/wave/stellar)

---

## Overview

LumenCredit is an open-source decentralized micro-lending platform built on the Stellar blockchain using Soroban smart contracts. It enables individuals — especially those without access to traditional banking — to borrow and lend USDC, and build a verifiable on-chain credit history over time.

Every repayment is recorded immutably on Stellar. Over time, this record becomes a portable credit score that borrowers can carry across platforms, borders, and lenders — no bank account required.

**The problem:** 1.4 billion adults globally are unbanked. Without a credit history, they cannot access loans. Without loans, they cannot grow. Traditional credit systems exclude them by design.

**Our solution:** Use Stellar's fast, low-cost finality to write repayment events on-chain. A borrower's history becomes a tamper-proof, globally readable credit score — owned by them, not a bureau.

---

## Features

- **Soroban loan contracts** — Borrow and repay USDC through auditable smart contracts with configurable terms (duration, interest rate, collateral)
- **On-chain credit score** — Every repayment updates a borrower's credit score NFT/SBT, readable by any lender on the network
- **Lender pools** — Lenders stake USDC into community pools; interest earnings are distributed proportionally
- **Oracle-based interest rates** — Dynamic rates informed by repayment history and pool liquidity
- **Borrower dashboard** — Track active loans, repayment schedule, score history, and available credit lines
- **KYC / DID integration** — Optional identity attestation via W3C Verifiable Credentials for compliant lending
- **Revocation & dispute module** — On-chain mechanism for flagging fraudulent activity

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        Frontend (Next.js)                    │
│         Borrower Dashboard │ Lender Portal │ Score Explorer  │
└───────────────────────────┬─────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                    Soroban Smart Contracts                    │
│                                                             │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────┐  │
│  │  Loan Pool   │  │  Credit Score│  │  Repayment       │  │
│  │  Contract    │  │  Contract    │  │  Tracker         │  │
│  └──────────────┘  └──────────────┘  └──────────────────┘  │
└───────────────────────────┬─────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                      Stellar Network                         │
│         USDC Transfers │ Ledger Events │ Account Trustlines  │
└─────────────────────────────────────────────────────────────┘
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| Blockchain | Stellar (Mainnet / Testnet) |
| Smart contracts | Soroban (Rust) |
| Frontend | Next.js + TypeScript |
| Wallet | Freighter, Albedo |
| Stablecoin | USDC on Stellar |
| Identity (optional) | W3C DID / Verifiable Credentials |
| Indexer | Stellar Horizon API |

---

## Getting Started

### Prerequisites

- [Rust](https://www.rust-lang.org/tools/install) (latest stable)
- [Soroban CLI](https://soroban.stellar.org/docs/getting-started/setup)
- [Node.js](https://nodejs.org/) v18+
- [Freighter Wallet](https://freighter.app/) browser extension

### Installation

```bash
# Clone the repository
git clone https://github.com/your-org/lumencredit.git
cd lumencredit

# Install frontend dependencies
cd frontend && npm install

# Build Soroban contracts
cd ../contracts
cargo build --target wasm32-unknown-unknown --release
```

### Deploy contracts to Stellar Testnet

```bash
soroban contract deploy \
  --wasm target/wasm32-unknown-unknown/release/loan_pool.wasm \
  --network testnet \
  --source <YOUR_SECRET_KEY>
```

### Run the frontend

```bash
cd frontend
cp .env.example .env.local
# Fill in your contract addresses and Horizon endpoint
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) to view the app.

---

## Smart Contract Overview

### `loan_pool` contract

Manages USDC deposits from lenders and disbursements to borrowers.

```rust
// Key public functions
fn deposit(env: Env, lender: Address, amount: i128);
fn request_loan(env: Env, borrower: Address, amount: i128, duration_days: u32);
fn repay(env: Env, borrower: Address, loan_id: u64, amount: i128);
fn withdraw(env: Env, lender: Address, amount: i128);
```

### `credit_score` contract

Reads repayment history and computes a score (0–1000) for each borrower address.

```rust
fn get_score(env: Env, borrower: Address) -> u32;
fn record_repayment(env: Env, borrower: Address, on_time: bool);
fn get_history(env: Env, borrower: Address) -> Vec<RepaymentEvent>;
```

---

## Roadmap

- [x] Project scaffolding & contract design
- [ ] `loan_pool` Soroban contract (v1)
- [ ] `credit_score` Soroban contract (v1)
- [ ] Borrower dashboard (Next.js)
- [ ] Lender staking portal
- [ ] Testnet launch
- [ ] Security audit
- [ ] Mainnet launch
- [ ] Mobile-optimized PWA
- [ ] DID / KYC integration

---

## Contributing

We welcome contributors of all experience levels. This project is part of the **Stellar Wave Program** — open issues are tagged and rewarded.

1. Fork the repo
2. Create a feature branch: `git checkout -b feat/your-feature`
3. Commit your changes: `git commit -m 'feat: add your feature'`
4. Push and open a Pull Request

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

### Good first issues

Look for issues tagged `good first issue` or `stellar-wave` in the GitHub Issues tab.

---

## License

MIT © LumenCredit Contributors
