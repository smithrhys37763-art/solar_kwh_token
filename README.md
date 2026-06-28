# solar_kwh_token

## Project Title
solar_kwh_token

## Project Description
`solar_kwh_token` is a Soroban smart contract on the Stellar network that tokenizes real-world solar energy production. Households and prosumers (producer-consumers) register a solar installation on-chain, an authorised oracle submits the kWh generated for each reporting period, and the producer mints kWh tokens into their balance — each token representing one unit of measured, verifiable energy. Consumers then call `redeem` to record off-take, moving the kWh tokens from the producer's balance into theirs. The result is a transparent, auditable ledger of distributed solar production and consumption, with no real XLM transfer required.

## Project Vision
Our vision is to unlock a peer-to-peer, transparent solar energy economy on Stellar. Every kilowatt-hour produced by a rooftop or community solar installation should be verifiable on-chain and tradeable as a digital asset, removing the need for opaque utility intermediaries. By turning energy production into transferable tokens, we aim to enable new financing models for distributed renewables, simplify carbon-accounting and renewable-energy-certificate (REC) issuance, and bring retail energy participants onto the same low-cost, fast-settlement layer as the rest of the digital economy — with the long-term goal of making clean energy a first-class, programmable asset class.

## Key Features
- **Installation Registry** — A producer registers a unique `install_id` with their nameplate `capacity_kw`, a hashed location reference (`location_hash`) for privacy, and the address of an authorised `oracle`. Each installation is tracked independently on-chain.
- **Oracle-Reported Generation** — A trusted oracle address submits the kWh produced for a given `period` (e.g. `2024Q1`, `202406`). Duplicate reports for the same `(install_id, period)` pair are rejected, preserving an immutable audit trail.
- **Tokenised kWh Minting** — The producer mints kWh tokens into their balance up to the total reported (but un-minted) generation, so every token is anchored in real measured output rather than凭空 created.
- **Consumer Off-Take (Redeem)** — A consumer calls `redeem(consumer, install_id, kwh)` to record off-take: tokens move from the producer's balance into the consumer's balance, and a per-consumer off-take tally is updated for analytics.
- **On-Chain Views** — `get_balance` and `is_valid_installation` let any client (wallet, dashboard, indexer) query the registry and individual holdings without requiring authentication.
- **Defensive Storage Model** — A tagged `DataKey` enum isolates installations, reported periods, balances, and per-consumer off-take tallies, preventing key collisions and keeping lookups O(1).

## Contract

- **Network:** Stellar Testnet (Public)
- **Scope:** environment dApp — see `contracts/solar_kwh_token/src/lib.rs` for the full solar_kwh_token business logic.
- **Functions exposed:** see `Key Features` above and the `pub fn` list in `lib.rs`.
- **Contract ID:** `CCMDYB2X3ICLIPSEGBUCFY7ENQL7XVPY6L2GWEK6GCPEUILQK3TAHUCF`
- **Explorer template:** `https://stellar.expert/explorer/testnet/tx/c19432ff71b72b4a3342338d48377589729bda2801e74477a86356b5db5575ae`

## Future Scope
- **Pluggable, Multi-Oracle Support** — Allow an installation to whitelist several oracle addresses with optional weighted aggregation, reducing single-points-of-failure in generation reporting.
- **Persistent Storage & TTL Bumps** — Migrate balances, reports, and per-consumer tallies from instance storage to persistent storage with explicit `extend_ttl` calls so that long-lived ledgers survive across many ledgers.
- **Direct Producer → Consumer Transfer** — Add an explicit `transfer` function so producers can sell kWh to specific consumers in a single call, optionally attaching an external payment reference.
- **Stablecoin Settlement Integration** — Wire the contract into Stellar USDC trustlines so that off-chain fiat settlement can be tied on-chain to redeemed kWh for pay-for-production flows.
- **Carbon-Credit Bridge** — Mint a parallel "carbon credit" token pegged to redeemed kWh (e.g. 1 credit = 1000 kWh redeemed) for voluntary carbon markets and ESG reporting.
- **React / Next.js Frontend** — Build a Freighter-wallet-connected dApp so producers can register installations, view oracle reports, mint tokens, and let consumers redeem — all from a browser.
- **Comprehensive Test Suite** — Add `#[test]` cases for the happy path plus all panic branches (double-registration, unauthorised oracle, over-mint, insufficient balance, inactive installation).

## Profile

- **Name:** <!-- Fill github name -->
- **Project:** `solar_kwh_token` (environment)
- **Built with:** Soroban SDK 25, Rust, Stellar Testnet
