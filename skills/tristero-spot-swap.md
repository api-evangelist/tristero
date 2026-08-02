---
name: Execute a cross-chain spot swap on Tristero
description: >-
  Quote and execute an EVM-to-EVM spot swap of ERC-20 tokens on the Tristero
  API using Permit2 / EIP-712 signed orders.
api: openapi/tristero-api-openapi.json
operations: [getQuote, fillOrder]
generated: '2026-07-21'
method: generated
---

# Execute a cross-chain spot swap on Tristero

Base URL: `https://api.tristero.com/v2` (staging: `https://staging-api.tristero.com/v2`). Send `X-API-Key` only if your deployment requires it (see `authentication/tristero-authentication.yml`).

1. **Quote** — `POST /quotes` (`getQuote`) with source and destination chain/token and the raw amount. The response carries `OrderData` (with `OrderParameters`) ready for signing. A `422` means the route is not available; `400` means invalid parameters — the error envelope is `{error, detail}`.
2. **Sign** — build the EIP-712 typed data (`EIP712Domain` + `PermitWitnessTransferFrom` over `TokenPermissions` and the `SignedOrder`) and sign with the trader's wallet key. Permit2 makes approval gasless — no on-chain approval transaction is needed.
3. **Submit** — `POST /orders` (`fillOrder`) with the `Permit2OrderSubmission`. `400` = invalid order data.
4. **Monitor** — order updates stream over WebSocket (Python SDK `wait_for_completion`); quotes go stale quickly (~500ms streaming cadence), so sign and submit promptly or subscribe to live quotes.

Rules: there is **no idempotency-key contract** — do not blind-retry `fillOrder` after a timeout; check order state first. No pagination or rate-limit headers are documented (`conventions/tristero-conventions.yml`).
