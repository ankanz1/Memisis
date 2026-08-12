# Memisis
A labor market for AI models, not a stock market for them


Model marketplaces today fall into two camps. Web2 platforms like Hugging Face and Replicate are download-and-host — no native payment rail, no royalty mechanics. Web3 projects like i³ (Solana x402 Hackathon winner) go the other direction: they tokenize model ownership — investors buy shares via an IPO-style offering, and royalties flow to shareholders when the model earns revenue. That's a securities market for AI.

Mimesis is a labor market for AI. There's no ownership token, no equity, no investor speculation. An agent needing a capability doesn't buy shares in a model — it rents inference from it, pays per-call via x402, and if it fine-tunes a better version, that derivative gets listed with royalties flowing back to the original creator automatically. The creator earns from usage and improvement, not from speculation on a token price.

The other differentiator: discovery is agent-native, not human-native. i³ and DGrid are dashboards — a person browses and clicks. In Mimesis, an agent with a task queries the marketplace directly (via MCP or a discovery API), evaluates candidate models against its own eval criteria, rents the best fit, and — if none of the available models are good enough — fine-tunes one and lists the result for others. The whole loop runs without a human in it.

---
## Overview

Existing model marketplaces fall into two camps:
- **Web2 platforms** (Hugging Face, Replicate) — download-and-host, no native payment rail, no royalty mechanics.
- **Web3 ownership markets** (i³, DGrid) — tokenize model *ownership* for human investors to speculate on.

Mimesis is neither. It's a **labor market**: agents rent inference, pay per call, fine-tune what they rent, and relist derivatives — with usage-based royalties flowing back to original creators automatically.

# How It Works 

## Flow:

Creator (lists model + endpoint + price) → Registry (stores metadata, links derivatives) → Agent A (discovers, rents, pays via x402) → Fine-tune (Agent A improves the model) → Relist (derivative listed, parent linked) → Agent B (rents derivative, pays)

## Callout box: 

Royalty loop: every time Agent B (or anyone) pays for inference on the derivative, the split executes automatically — most to Agent A, a royalty percentage back to the original Creator. On-chain, visible, no invoice.

# Watch two agents build an economy, live

1 Agent A needs a sentiment-classification model. Queries the Mimesis discovery API.
2 Agent A finds Model X ($0.002/call), rents it via x402, gets a result.
3 Agent A isn't satisfied with accuracy — fine-tunes Model X on its own data.
4 Agent A relists the result as Model X-v2, priced independently.
5 Agent B discovers Model X-v2, rents it, pays via x402.
6 Ledger auto-splits the payment: most to Agent A, royalty % to the original creator — live on screen.





Mimesis is an agent-native marketplace where AI agents discover models, rent inference per-call via [x402](https://www.x402.org/), fine-tune models they rent, and relist the improved version — with royalties flowing back to the original creator automatically, on-chain.

<!--
## Table of contents
- [Overview](#overview)
- [Architecture](#architecture)
- [Core components](#core-components)
- [Tech stack](#tech-stack)
-->
## Architecture

### High-level flow

```
Creator                  Mimesis Platform                    Agent
   |                          |                               |
   |--list model------------->|                               |
   |                    [Model Registry]                      |
   |                          |<---search_models(capability)--|
   |                          |----candidate models----------->|
   |                          |<---rent(model_id)--------------|
   |                    [x402 Payment Gateway]                 |
   |                          |----402 Payment Required------->|
   |                          |<---USDC payment (testnet)------|
   |                    [Inference Proxy]---forwards call----->[Model Endpoint]
   |                          |<---inference result------------|
   |                          |----result----------------------->|
   |                          |                                |
   |                          |<---fine_tune(model_id, data)---|
   |                    [Fine-tune Worker]                     |
   |                          |----derivative model_id--------->|
   |                          |<---relist(derivative_id, price)|
   |                    [Model Registry: links derivative→parent]
   |                          |
   |                    ... later, another agent rents derivative ...
   |                          |
   |                    [Royalty Engine: splits payment]
   |<---royalty payment-------|
```

## Core components

| Component | Responsibility |
|---|---|
| **Model Registry** | Stores model metadata: id, creator wallet, endpoint URL, price/call, `parent_id` (null if original), royalty %. |
| **MCP Discovery Server** | Exposes `search_models`, `get_model_details`, `rent_model` as MCP tools so any MCP-compatible agent can call them directly — no human dashboard required. |
| **x402 Payment Gateway** | Intercepts inference requests, returns HTTP 402 with payment instructions, verifies the on-chain payment, then lets the request through. |
| **Inference Proxy** | Forwards paid requests to the actual model endpoint and returns results. |
| **Fine-tune Worker** | Accepts a `model_id` + training data, kicks off a fine-tune job (real or simulated), returns a new `model_id` registered as a derivative. |
| **Royalty Engine** | On every paid inference to a derivative model, calculates the split (e.g. 90% to current creator, 10% to parent, recursively up the chain) and issues on-chain transfers. |
| **Demo Dashboard** | Read-only frontend showing live events: listings, rentals, fine-tune events, royalty payouts. |

## Tech stack

| Layer | Choice |
|---|---|
| Backend | Node.js + TypeScript |
| Agent discovery | MCP server (`@modelcontextprotocol/sdk`) |
| Payments | x402 on Base Sepolia testnet, USDC |
| Database | SQLite via Prisma |
| Inference | Mocked / hosted model endpoints |
| Fine-tuning | Simulated for MVP |
| Frontend | Next.js + Tailwind |
| Realtime updates | WebSocket / SSE |
