# DeskPulse

Paid MCP server for crypto market intel. Agents get Kraken and Hyperliquid order book depth, funding and spot–futures basis, and pay per tool call with x402 USDC on Base.

- **MCP endpoint (Streamable HTTP):** `https://deskpulse.mubado.cloud/mcp`
- **Website:** https://deskpulse.mubado.cloud
- **Agent docs:** https://deskpulse.mubado.cloud/llms.txt
- **MCP Registry:** `io.github.mubado/deskpulse`

This repo is the public listing for DeskPulse. It's a hosted remote server, so there's nothing to install or run locally.

## Tools

| Tool | Price (USDC) | What it returns |
|------|--------------|-----------------|
| `health` | free | Service status, version, network and payTo |
| `book_depth` | $0.03 | Kraken spot order book depth |
| `funding_snapshot` | $0.05 | Kraken Futures funding rates |
| `basis_snapshot` | $0.05 | Kraken spot vs futures basis |
| `hl_book_depth` | $0.03 | Hyperliquid L2 order book |
| `hl_funding_snapshot` | $0.05 | Hyperliquid funding rates |

## Connect

Add the remote server to any MCP client that supports Streamable HTTP:

```json
{
  "mcpServers": {
    "deskpulse": {
      "url": "https://deskpulse.mubado.cloud/mcp"
    }
  }
}
```

Call `health` first to check the connection. It's free.

## Payment

Paid tools use [x402](https://www.x402.org). An unpaid call returns HTTP 402 with the price and payment details. An x402-capable client signs a USDC payment on Base, retries, and gets the result. Settlement goes through the Coinbase CDP facilitator.

- Network: Base mainnet (`eip155:8453`)
- Asset: USDC
- payTo (receive-only): `0x51B8Cbcb75798CE7deD1BE9050c144907541a00B`

The server holds no private keys.

## Data sources

Public market data from Kraken (spot and Futures) and Hyperliquid, fetched live per call.

## Disclaimer

Market data is for information only and isn't financial advice. Data can be delayed or incomplete when upstream venues are slow or unavailable.
