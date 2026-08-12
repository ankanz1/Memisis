# Memisis
A labor market for AI models, not a stock market for them


Model marketplaces today fall into two camps. Web2 platforms like Hugging Face and Replicate are download-and-host — no native payment rail, no royalty mechanics. Web3 projects like i³ (Solana x402 Hackathon winner) go the other direction: they tokenize model ownership — investors buy shares via an IPO-style offering, and royalties flow to shareholders when the model earns revenue. That's a securities market for AI.

Mimesis is a labor market for AI. There's no ownership token, no equity, no investor speculation. An agent needing a capability doesn't buy shares in a model — it rents inference from it, pays per-call via x402, and if it fine-tunes a better version, that derivative gets listed with royalties flowing back to the original creator automatically. The creator earns from usage and improvement, not from speculation on a token price.

The other differentiator: discovery is agent-native, not human-native. i³ and DGrid are dashboards — a person browses and clicks. In Mimesis, an agent with a task queries the marketplace directly (via MCP or a discovery API), evaluates candidate models against its own eval criteria, rents the best fit, and — if none of the available models are good enough — fine-tunes one and lists the result for others. The whole loop runs without a human in it.

---

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

# High-level flow

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
