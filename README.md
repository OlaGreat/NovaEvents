# NovaEvents

> A Soroban smart contract platform for transparent event management on Stellar — featuring sponsorship funding, multi-tier ticketing, and automated payouts, all settled and verifiable on-chain.

## Overview

NovaEvents brings end-to-end transparency to event management. Every financial action — sponsor contributions, ticket sales, and post-event payouts — happens on-chain, so all stakeholders can see exactly how money flows through an event.

The core idea: events lose trust when funding and spending happen behind closed doors. Here, sponsors can see what every other sponsor contributed, attendees know their tickets are genuinely owned and verifiable, and everyone can trace how the collected funds were ultimately spent. There are no hidden ledgers — the contract *is* the ledger.

The platform supports:

- **Event organizers** create and manage events with funding goals and multiple ticket tiers
- **Sponsors** contribute to event funding with full visibility into every other sponsor's contribution
- **Attendees** purchase tickets across price tiers, with on-chain proof of ownership
- **Workers** receive payouts directly through the platform once the event concludes
- **Everyone** benefits from transparent, trustless settlement — funds move on-chain and are auditable by anyone

NovaEvents is a Stellar-native rewrite of a prior version originally built and deployed on Lisk Sepolia (Solidity/Foundry). The concept, architecture, and role model are proven; this version reimplements them on Soroban to bring the platform to the Stellar ecosystem.

## Why this matters

Event funding today is opaque. Sponsors hand over money and rarely see how it's used or what others gave. Attendees buy tickets they can't independently verify. Workers chase payments after the event.

By moving the entire money flow on-chain, this platform turns an event into a transparent, auditable process:

- Sponsors contribute publicly and can see the full sponsorship picture in real time.
- Ticket sales settle on-chain, so revenue is visible, not self-reported.
- Payouts happen through the platform, so spending is traceable to the same ledger that collected the funds.

It also opens a door for the wider community: anyone whose work revolves around events — organizers, promoters, vendors, ticketing tools — has a transparent, programmable settlement layer to build on top of.

## Architecture

The platform is built around a Soroban smart contract (Rust) that holds event state and enforces the rules of funding, sales, and payouts. The contract is the single source of truth; clients (CLI, scripts, or a future frontend) read from and write to it through the Stellar network.

### Core concepts

- **Event** — created by an organizer; holds metadata, a funding goal, ticket tiers, and the current balance of collected funds.
- **Ticket tier** — a named price level (e.g. General, VIP) with its own price and supply cap.
- **Ticket** — an on-chain ownership record tied to a buyer's address and a tier. Used for entry verification and (optionally) transfer.
- **Sponsorship** — a public contribution to an event's funding, recorded against the sponsor's address so all contributions are visible.
- **Payout** — a disbursement of collected funds to a recipient (e.g. a worker), recorded on-chain so spending is auditable.

### Roles

| Role | Can do |
|------|--------|
| Organizer | Create events, define tiers, check in tickets, trigger payouts |
| Sponsor | Contribute funds to an event; view all sponsorships |
| Attendee | Buy tickets; hold and verify ownership; (optionally) transfer |
| Worker | Receive payouts |
| Anyone | Read event state, sponsorships, sales, and payouts |

## Project scope

This repository is built and contributed to in stages. The **core** is implemented first and deployed to Stellar testnet; the remaining features are tracked as open issues for contributors.

### Core (implemented + deployed to testnet)

- Event creation with funding goal and one or more ticket tiers
- Ticket purchase in USDC, settled on-chain, producing an ownership record
- Check-in / redeem (organizer marks a ticket as used)
- Sponsorship contributions in USDC, recorded publicly per sponsor
- Read access to event state, sponsorships, and ticket records

### Planned (open for contribution)

These are intentionally scoped as contributor issues:

- Proportional revenue shares for sponsors based on contribution size
- Automated payroll distribution to workers after an event concludes
- Ticket transfer and resale rules (price caps, organizer royalties)
- QR-code-based check-in flow
- A web frontend (organizer dashboard, sponsor view, attendee ticket wallet)
- Event lifecycle controls (cancel event, refund logic)
- TypeScript bindings / SDK for the contract

If you're a contributor looking for where to start, check the **Issues** tab — each issue is scoped with clear acceptance criteria.

## Tech stack

- **Smart contracts:** Rust + [Soroban](https://soroban.stellar.org/)
- **Network:** Stellar (testnet for development)
- **Tooling:** Stellar CLI, `soroban-sdk`
- **Settlement token:** USDC on Stellar

## Getting started

> Setup steps and exact commands will be finalized alongside the core contract. The flow below describes the intended developer experience.

### Prerequisites

- [Rust](https://www.rust-lang.org/tools/install) with the `wasm32-unknown-unknown` target
- [Stellar CLI](https://developers.stellar.org/docs/build/smart-contracts/getting-started/setup)
- A funded Stellar testnet account

### Build

```bash
stellar contract build
```

### Test

```bash
cargo test
```

### Deploy to testnet

```bash
stellar contract deploy \
  --wasm target/wasm32-unknown-unknown/release/event_ticketing.wasm \
  --network testnet
```

The deployed contract ID will be published here once available.

## Contributing

Contributions are welcome. This project is open to developers, designers, and product builders who want to help bring transparent event infrastructure to Stellar.

Please read [CONTRIBUTING.md](./CONTRIBUTING.md) before opening a pull request, and browse the open issues for scoped tasks. Each issue describes what "done" looks like, so you know exactly what to build before you start.

## License

MIT
