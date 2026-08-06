# Memisis
A labor market for AI models, not a stock market for them


Model marketplaces today fall into two camps. Web2 platforms like Hugging Face and Replicate are download-and-host — no native payment rail, no royalty mechanics. Web3 projects like i³ (Solana x402 Hackathon winner) go the other direction: they tokenize model ownership — investors buy shares via an IPO-style offering, and royalties flow to shareholders when the model earns revenue. That's a securities market for AI.

Mimesis is a labor market for AI. There's no ownership token, no equity, no investor speculation. An agent needing a capability doesn't buy shares in a model — it rents inference from it, pays per-call via x402, and if it fine-tunes a better version, that derivative gets listed with royalties flowing back to the original creator automatically. The creator earns from usage and improvement, not from speculation on a token price.

The other differentiator: discovery is agent-native, not human-native. i³ and DGrid are dashboards — a person browses and clicks. In Mimesis, an agent with a task queries the marketplace directly (via MCP or a discovery API), evaluates candidate models against its own eval criteria, rents the best fit, and — if none of the available models are good enough — fine-tunes one and lists the result for others. The whole loop runs without a human in it.
