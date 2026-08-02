# Tristero

Tristero is a trustless, cross-chain trading protocol backed by General Catalyst. Traders use its API and Python/TypeScript SDKs for spot swaps of ERC-20 tokens across EVM chains (Permit2 / EIP-712 signed orders), leveraged margin positions up to 10x, and cross-VM swaps into non-EVM assets like Bitcoin, Monero, and Litecoin via the Feather balance-sheet swap relay. Execution is non-custodial and MEV-protected, with real-time quote streaming over WebSocket.

- Website: https://tristero.com
- Developer docs: https://docs.tristero.com
- App: https://app.tristero.com
- GitHub: https://github.com/tristeroresearch

## APIs

- **Tristero API** (`https://api.tristero.com/v2`) — spot swap quotes, signed order submission, margin positions. OpenAPI: `openapi/tristero-api-openapi.json`
- **Feather API** (`https://feather-prod.tristero.com`) — unauthenticated cross-VM swap relay (assets, price, trade, status). OpenAPI: `openapi/tristero-feather-api-openapi.json`

Backed by: general-catalyst
