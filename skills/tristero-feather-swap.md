---
name: Swap into non-EVM assets with Feather
description: >-
  Execute a cross-VM swap (e.g. ETH to BTC, or into XMR/LTC/SOL) through
  Tristero's Feather relay - an unauthenticated HTTP/JSON deposit-address
  flow with no smart contracts or wallet connections.
api: openapi/tristero-feather-api-openapi.json
operations: [getAssets, getPrice, createTrade, getStatus]
generated: '2026-07-21'
method: generated
---

# Swap into non-EVM assets with Feather

Base URL: `https://feather-prod.tristero.com`. No authentication required.

1. **Discover** — `GET /assets` (`getAssets`) lists supported assets (Bitcoin, Monero, Ethereum, USDC, Litecoin, Solana) with live bid/ask, min amounts, and per-asset liquidity limits (`MaxInput`/`MaxOutput`). Respect these limits — Feather pays from its own reserves.
2. **Price** — `GET /price` (`getPrice`) with input and output assets; optionally pass an amount for a worked example.
3. **Trade** — `POST /trade` (`createTrade`) with input/output assets, the destination address, and a return address. The response allocates a **deposit address** and a trade id. `400` = invalid parameters (e.g. malformed output address); envelope is `{result: "error", error}`.
4. **Deposit** — send funds to the returned deposit address before the trade expires. This is a plain on-chain transfer; there is no wallet connection.
5. **Poll** — `GET /status` (`getStatus`) with the trade id until the state machine reaches finalized (`Deposited` -> `Sent` -> `Finalized`; `FailedAndReturned` signals a refund to the return address). `404` = unknown trade id.

Rules: quotes move with Feather's balance-sheet pricing — re-check `getPrice` if the deposit is delayed. Always supply a return address so failed trades refund automatically.
