<p align="center">
  <a href="https://drive.google.com/drive/folders/1N8dIMTGrTn7qD_2kmd9MyhxB42SoPIhE?usp=sharing">
    <img src="./assets/sumplus-logo.png" alt="Sumplus Logo" width="120" />
  </a>
</p>

<h1 align="center">Sumplus Infrastructure</h1>

<p align="center">
  <strong>The Composable Financial Stack for AI Agents</strong>
</p>

<p align="center">
  <a href="https://arsenal.sumplus.xyz">Arsenal</a> •
  <a href="https://arsenal.sumplus.xyz/mcp">MCP Endpoint</a> •
  <a href="https://maria.sumplus.xyz">Maria</a> •
  <a href="https://www.sumplus.xyz">Website</a> •
  <a href="https://x.com/SumplusReal">Twitter</a> •
  <a href="https://t.me/sumplus_official">Telegram</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Skills-70%2B-blue" alt="Skills" />
  <img src="https://img.shields.io/badge/Chains-12%2B-green" alt="Chains" />
  <img src="https://img.shields.io/badge/Protocol-MCP%20%26%20REST-purple" alt="MCP" />
  <img src="https://img.shields.io/badge/License-MIT-yellow" alt="License" />
</p>

---


## What is Sumplus?

Sumplus is building the composable financial stack for AI agents. Our vision is to make financial capabilities secure, callable, and easy for agents to use — so AI can not only reason about capital, but also act on it safely on-chain.

The stack consists of three core products:

- **Arsenal** — An open skill marketplace with 70+ composable DeFi skills, callable by AI agents via MCP (STDIO) or HTTP
- **Maria** — An on-chain AI financial agent with verifiable identity on the Metaplex Agent Registry
- **Yield** — Agent-ready yield strategies, delivered today as the `sumplus-yield` skill in Arsenal; standalone product roadmapped

## Why Sumplus?

Every AI agent team building on-chain financial capabilities faces the same problem: integrating with DeFi protocols is slow, fragile, and repetitive. Each protocol requires custom integration work — API wrappers, wallet management, chain-specific edge cases, error handling.

Arsenal eliminates this. One catalog. Every skill. Every chain.

```
HTTP REST:        https://arsenal.sumplus.xyz/api/execute
JSON-RPC over HTTP: https://arsenal.sumplus.xyz/mcp
MCP STDIO server:   bundled (see Quick Start)
```

## Architecture

```
┌─────────────────────────────────────────────────┐
│                   Your Agent                     │
│          (Claude, Cursor, custom LLM)            │
└──────────────────────┬──────────────────────────┘
                       │ MCP (STDIO) / REST / JSON-RPC
                       ▼
┌─────────────────────────────────────────────────┐
│              Sumplus Arsenal                     │
│         Skill Discovery & Invocation             │
│                                                  │
│  ┌──────────┐ ┌──────────┐ ┌──────────────────┐ │
│  │ DEX      │ │ Lending  │ │ Yield Strategies │ │
│  │ Skills   │ │ Skills   │ │ Skills           │ │
│  └──────────┘ └──────────┘ └──────────────────┘ │
│  ┌──────────┐ ┌──────────┐ ┌──────────────────┐ │
│  │ Perps    │ │ Bridge   │ │ Prediction Mkt   │ │
│  │ Skills   │ │ Skills   │ │ Skills           │ │
│  └──────────┘ └──────────┘ └──────────────────┘ │
│  ┌──────────┐ ┌──────────┐ ┌──────────────────┐ │
│  │ Data     │ │ Wallet   │ │ Agent Infra      │ │
│  │ Skills   │ │ Tools    │ │ Skills           │ │
│  └──────────┘ └──────────┘ └──────────────────┘ │
└──────────────────────┬──────────────────────────┘
                       │
   ┌─────┬─────┬─────┬─┴─┬─────┬─────┬──────┐
   ▼     ▼     ▼     ▼   ▼     ▼     ▼      ▼
  Sui  Eth  Solana Base BNB Avalanche Arb HashKey  ...
```

## Skill Categories

| Category | Protocols | Skills |
|---|---|---|
| **Spot DEX & Trading** | Uniswap, Jupiter, Raydium, 1inch, PancakeSwap, Aerodrome, Cetus, Aftermath, Velodrome, Curve, QuickSwap | Swap execution, quote queries, route optimization |
| **Perpetuals & Derivatives** | Hyperliquid, GMX, dYdX | Open/close positions, funding rates, hedging |
| **Cross-Chain Bridges** | Wormhole, Across, Stargate, DeBridge, CCTP | Cross-chain transfers, fee queries, route selection |
| **Prediction Markets** | Polymarket | Market queries, position execution, probability analysis |
| **Data & Intelligence** | DefiLlama, CoinGecko, Chainlink, Pyth, The Graph | TVL, prices, oracle data, yield analytics |
| **Wallet & On-Chain Tools** | Wallet Manager, SUI Toolkit, Token Lists, ETH RPC, Tenderly | Wallet ops, balance checks, tx simulation |
| **Lending & Borrowing** | Aave, Moonwell, Morpho, Scallop, Suilend, Navi | Supply, borrow, health factor monitoring |
| **Yield Strategies** | Sumplus Yield, Pendle, Ethena, Lido, Bluefin Ember, XPower | Yield discovery, deposits, RWA strategies |
| **Agent Infrastructure** | Sumplus Arsenal, Talus Nexus | A2A coordination, verified execution |
| **Blockchain** | Sui, Solana, Ethereum, Base, BNB Chain, Avalanche, HashKey, X Layer | Chain SOPs, RPC configs, gas management |

## Quick Start

Arsenal exposes its skill catalog through three connection paths. Pick whichever fits your agent.

### Option A — HTTP REST (any client)

The simplest path. Works from any language that can do HTTPS.

```bash
# 1. Discover skills
curl -s "https://arsenal.sumplus.xyz/api/skills?q=jupiter+swap"

# 2. Execute by skill_id
curl -X POST https://arsenal.sumplus.xyz/api/execute \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "skill_id": "<uuid-from-step-1>",
    "input": {
      "action": "get_quote",
      "token_in":  "USDC",
      "token_out": "SOL",
      "amount_in": "100",
      "slippage_bps": 50
    }
  }'
```

### Option B — JSON-RPC over HTTP at `/mcp`

For agents that prefer JSON-RPC. Stateless, no session, no auth required for read methods.

```bash
# Discover
curl -X POST https://arsenal.sumplus.xyz/mcp \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"skills/list","params":{"search":"jupiter swap","limit":5}}'

# Get one
curl -X POST https://arsenal.sumplus.xyz/mcp \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":2,"method":"skills/get","params":{"id":"<uuid>"}}'

# Execute
curl -X POST https://arsenal.sumplus.xyz/mcp \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":3,"method":"skills/execute","params":{"id":"<uuid>","input":{...}}}'
```

Supported methods: `skills/list`, `skills/get`, `skills/execute`.

### Option C — MCP STDIO server (Claude Desktop / Cursor)

For real MCP clients, Arsenal ships a STDIO MCP server. Clone the Arsenal repo locally, then add this to your MCP client config:

```json
{
  "mcpServers": {
    "arsenal": {
      "command": "node",
      "args": ["/path/to/sumplus-arsenal/bin/mcp-server.js"],
      "env": {
        "NEXT_PUBLIC_SUPABASE_URL": "<arsenal-supabase-url>",
        "SUPABASE_SERVICE_ROLE_KEY": "<arsenal-supabase-key>",
        "OPENAI_BASE_URL": "<embedding-api-url>",
        "OPENAI_API_KEY": "<embedding-api-key>"
      }
    }
  }
}
```

Once configured, Claude / Cursor will see Arsenal's skills as native MCP tools.

> **Roadmap:** Streamable HTTP MCP transport (zero-config, no local clone needed) is on the near-term roadmap so that `Claude Desktop` and `Cursor` can connect to Arsenal directly via URL.

### Publish Your Own Skill

Arsenal is an open marketplace. Any developer can publish a skill:

```
1. Visit https://arsenal.sumplus.xyz/integrate
2. Define your skill schema (inputs, outputs, chain, category)
3. Submit for review
4. Once approved, your skill is discoverable by every agent on Arsenal
```

## Maria: The Reference Agent

Maria is Sumplus's own AI agent — and the first consumer of Arsenal skills. She demonstrates the full stack in production:

- **On-chain identity** registered on the [Metaplex Agent Registry](https://www.metaplex.com/agents/7qnonBq8KwhotSq9aZNoS5aqq17oPNpT9v6iohFe8Mnf)
- **Dedicated agent wallet** on Solana
- **x402-compatible** service endpoint
- **A2A execution**: User → Maria → Arsenal → On-chain

Maria earned **2nd place** at the Solana × Metaplex Hackathon. Metaplex called it *"Agent-to-agent commerce, live."*

Try Maria: [maria.sumplus.xyz](https://maria.sumplus.xyz)

## Supported Chains

| Chain | Status | Key Protocols |
|---|---|---|
| Sui | ✅ Live | Cetus, Aftermath, Scallop, Suilend, Navi, Bluefin, Talus Nexus |
| Ethereum | ✅ Live | Aave, Uniswap, Lido, Morpho, Pendle, Ethena, EigenLayer |
| Solana | ✅ Live | Jupiter, Raydium |
| Base | ✅ Live | Uniswap, Aerodrome, Moonwell, x402 (Coinbase facilitator) |
| BNB Chain | ✅ Live | PancakeSwap, x402 (Binance facilitator) |
| Avalanche | ✅ Live | x402 (Coinbase facilitator) |
| Arbitrum | ✅ Live | GMX, DeFi perps |
| HashKey Chain | ✅ Live | HSKSwap |
| X Layer | ✅ Live | Curve, Uniswap, QuickSwap |
| Tempo | ✅ Live | MPP (Machine Payments Protocol) |
| Hyperliquid | ✅ Live | Perps |
| Canton Global Synchronizer | 🔄 Read-only | Canton Coin / Splice (settlement M3 roadmapped) |

## CLARITY Act Alignment

Arsenal includes a dedicated [CLARITY Act section](https://arsenal.sumplus.xyz/skills?tag=clarity-act) — 16 DeFi skills tagged and aligned with the U.S. Digital Asset Market Clarity Act legislation, covering lending, yield, swaps, and liquid staking.

## Project Stats

| Metric | Value |
|---|---|
| Live Skills | 70+ |
| Skill Categories | 10 |
| Chains Supported | 12+ |
| Protocol Integrations | 40+ |
| Ecosystem Partners | 170+ |
| Community (X) | 20,000+ |
| Hackathon Recognition | 2nd Place, Solana × Metaplex |
| Backed by | Sui Ecosystem |

## Links

| Resource | URL |
|---|---|
| Arsenal (Skill Marketplace) | [arsenal.sumplus.xyz](https://arsenal.sumplus.xyz) |
| Arsenal `/mcp` JSON-RPC API | [arsenal.sumplus.xyz/mcp](https://arsenal.sumplus.xyz/mcp) |
| Maria (AI Agent) | [maria.sumplus.xyz](https://maria.sumplus.xyz) |
| CLARITY Act Skills | [arsenal.sumplus.xyz/skills?tag=clarity-act](https://arsenal.sumplus.xyz/skills?tag=clarity-act) |
| Website | [sumplus.xyz](https://www.sumplus.xyz) |
| Twitter | [@SumplusReal](https://x.com/SumplusReal) |
| Telegram | [sumplus_official](https://t.me/sumplus_official) |
| Medium | [medium.com/@sumplus_real](https://medium.com/@sumplus_real) |

## Contributing

Arsenal is an open skill marketplace. We welcome:

- **Skill submissions** — publish your protocol as a callable skill for AI agents at [arsenal.sumplus.xyz/integrate](https://arsenal.sumplus.xyz/integrate)
- **Integration feedback** — report issues or suggest improvements via GitHub Issues
- **Documentation PRs** — help improve docs and examples in this repository

## License

This project is licensed under the MIT License. See [LICENSE](./LICENSE) for details.

---

<p align="center">
  🔺 <strong>Sumplus — the execution layer the agent economy runs on.</strong>
</p>
