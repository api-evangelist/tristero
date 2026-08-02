---
name: Open, inspect, and close a margin position on Tristero
description: >-
  Take out a leveraged margin position (up to 10x) on the Tristero API, list
  and inspect positions for a wallet, and close a position with cash or swap
  settlement.
api: openapi/tristero-api-openapi.json
operations: [getMarginQuote, fillOrder, getMarginPositions, getMarginPosition, closeMarginPosition]
generated: '2026-07-21'
method: generated
---

# Open, inspect, and close a margin position on Tristero

Base URL: `https://api.tristero.com/v2`. Tristero margin is lending-based (no ADL): any token can serve as collateral.

1. **Quote** — `POST /quotes/margin` (`getMarginQuote`) with collateral token, target exposure, and leverage ratio. Response is a `MarginQuoteResponse` with signable `OrderData`. `400` = invalid request.
2. **Open** — sign the EIP-712 payload and submit via `POST /orders` (`fillOrder`) as a `MarginOrderSubmission`.
3. **List** — `GET /wallets/{wallet}/margin-positions` (`getMarginPositions`) returns all `MarginPosition` records for a wallet (no pagination — full list).
4. **Inspect** — `GET /wallets/margin-positions/{position_id}` (`getMarginPosition`) for one position: status, escrow address, collateral/loan amounts, leverage ratio, interest rate (bps).
5. **Close** — `POST /orders/close-margin-position` (`closeMarginPosition`) with a signed `ClosePositionSubmission`; partial or full close, cash settlement or swap settlement.

Rules: no idempotency keys — confirm position state via `getMarginPosition` before retrying a close. Errors use the `{error, detail}` envelope (`errors/tristero-problem-types.yml`).
