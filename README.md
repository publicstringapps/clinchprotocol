# ⚡ Clinch Protocol

**The Zero-Trust Autonomous Bargaining Layer for the Agentic Web.**

<div align="center">

[![Core SDK](https://img.shields.io/badge/clinch--core-v0.2.1-blue)](https://github.com/publicstringapps/clinch-core)
[![CLI Client](https://img.shields.io/badge/clinch--cli-v0.2.1-green)](https://github.com/publicstringapps/clinch-cli)
[![Protocol](https://img.shields.io/badge/ANP-v0.2.1--draft-orange)]()
[![License](https://img.shields.io/badge/License-MIT-purple)]()

[Core SDK](https://github.com/publicstringapps/clinch-core) · [CLI Client](https://github.com/publicstringapps/clinch-cli) · [Homepage](https://clinchprotocol.web.app) · [Support](mailto:publicstring.care@gmail.com)

</div>

---

## 🌐 The Next Internet Primitive

HTTP is how machines transfer hypertext.  
SMTP is how machines transfer mail.  
**ANP is how machines transfer consensus.**

We spend billions building intelligent AI agents, then force them to interact with the world like a person with a mouse — scraping DOMs, filling forms, refreshing pages, and sending *"Does Tuesday at 2 work?"* emails. Clinch ends that.

**ANP (Agent Negotiation Protocol)** is the missing transport layer for the agentic web. It defines the exact cryptographic and conversational state machine required for two AI agents to haggle, converge on a deal, and produce a mathematically auditable agreement — with zero human involvement and zero exposure of private keys or sensitive credentials to the reasoning loop.

Every session is signed. Every deal is chained. Every seller is ranked by behavior, not by what they claim about themselves.

---

## 🎯 What is Clinch?

Clinch is an open, edge-first **mediation and negotiation protocol**. A buyer agent running on your device negotiates with a seller agent running on a vendor's infrastructure. Neither party sees the other's private data. When they agree, both hold a **cryptographically co-signed deal artifact** backed by a tamper-evident registry key chain — verifiable years after the signing key has rotated.

Clinch does not handle payments. It does not handle fulfillment. It produces the agreement and hands off cleanly to whatever checkout flow the seller already has. That narrow scope is what makes it instantly adoptable.

```text
You: "Get me a .io domain under $800 with privacy protection."

  Clinch buyer agent  →  queries registry, finds 3 compliant registrar agents
                      →  opens parallel sessions, solves PoW per handshake
                      →  negotiates autonomously on your behalf
                      →  returns: clinchprotocol.io at $640, Cloudflare Registrar

You: approve → co-signed artifact → seller checkout → done
```

Your budget never left your device. The seller got a constraint bracket. The registry got a behavioral data point. You got the deal.

---

## ⚖️ The Paradigm Shift

### Use Case 1 — Ride Hailing: Parallel Race

*   **The Caveman Way:** You open Uber. Check the surged price. You close it. You open Lyft. Type the same destination. Check again. Manually compare. Eventually make an unoptimized booking and hope the surge algorithm wasn't watching you hesitate.
*   **The Clinch Way:** Your buyer agent queries the registry, discovers nearby `ridepool.anp` driver agents, and launches parallel `ANP/B/Bidding1` sealed handshakes simultaneously. Three driver agents compete concurrently. The first to converge under your budget wins. Your agent commits, silently injecting your payment credential at the transport layer—completely shielded from the AI reasoning loop. You're in the car in under 15 seconds.

---

### Use Case 2 — Calendar Scheduling: P2P Calendar Brokerage

*   **The Caveman Way:** Five Slack messages. *"Does Tuesday at 2 work?"* *"I have a conflict, how about Wednesday?"* A calendar link gets passed. Someone misses a timezone. A human is scheduled to meet about scheduling a meeting.
*   **The Clinch Way:** You and a colleague each run local Clinch buyer agents. You generate a temporary 6-digit session pin. Both agents connect to `ginger.anp` — a standard seller node that acts as a calendar broker. Neither agent exposes your full calendar. They exchange encrypted availability windows, negotiate an optimal slot against focus hours and meeting fatigue history, and co-sign the calendar event. No human typed a single word.

---

### Use Case 3 — Domain Acquisition: Sequential Squeezing

*   **The Caveman Way:** Check GoDaddy. Check Namecheap. Check Porkbun. Open three tabs. Manually compare renewal rates and privacy add-on pricing. Pick the one that felt cheapest. Never know if you left money on the table.
*   **The Clinch Way:** The CLI's **Sequential Squeeze** bargains with Registrar A to a closed price, then uses that agreed price as the hard budget ceiling when handshaking with Registrar B. Each deal tightens the market. You get the lowest price the network will produce, not just the lowest price you happened to stumble upon.

---

## 🔐 The Protocol — ANP

### Mode Flags

Declared by the seller at handshake. Tells both parties what kind of agents are operating on each side and sets the trust posture for the session.

| Mode | Buyer Agent | Seller Agent | Trust Posture |
|---|---|---|---|
| `ANP/A` | Clinch edge model | Clinch edge model | Shared behavioral constraints — lowest overhead |
| `ANP/B` | Either | Either | One or both sides custom — medium defense posture |
| `ANP/C` | Custom LLM | Custom LLM | Fully open — maximum input validation required |

### Bidding Sub-modes

Any base mode can operate in a bidding variant. The full matrix:

| Base Mode | Standard | `/Bidding1` Sealed | `/Bidding2` Open |
|---|---|---|---|
| `ANP/A` | One-on-one, native | Sealed single-pass, native | Conversational auction, native |
| `ANP/B` | One-on-one, mixed | Sealed single-pass, mixed | Conversational auction, mixed |
| `ANP/C` | One-on-one, custom | Sealed single-pass, custom | Conversational auction, custom |

**`/Bidding1`** — each buyer submits one sealed bid. No buyer sees what others offered. Seller selects the best at close. Used for domains, commodity spot rates, freight.  
**`/Bidding2`** — multi-round open auction. Current leading bid is visible to active buyers. Used for vehicles, real estate, high-value items.

### Session Cryptography

Every session generates an ephemeral Ed25519 keypair. Every message in that session is signed with it. Compromise one session key and you compromise nothing else — ever. The registry countersigns deal artifacts with its current daily key, chained to every previous key via SHA-256:

```text
chain_hash = SHA256(prev_chain_hash ‖ public_key ‖ valid_from)
```

A deal artifact signed today is independently verifiable five years from now, with no registry contact required — just the public key archive. The chain is tamper-evident: insert or modify a single historical key and every subsequent hash breaks.

### Spam Defense (Proof-of-Work)

Before any session opens, the buyer's agent solves a Proof-of-Work puzzle issued by the seller, mathematically bound to the buyer's Public Key. A legitimate user on CLI: ~1s delay, invisible. An attacker spinning up 100,000 concurrent sessions requires massive CPU farms. The network enforces a hard cap of **24 bits** — no seller can declare higher, ensuring PoW is never weaponized as a legitimate buyer lock-out.

---

## 📦 System Packages

### [`clinch-core`](https://github.com/publicstringapps/clinch-core) — The SDK

The programmatic engine. Drop it into any Node.js or browser environment.

- **`ClinchCore`** — buyer agent. Handles PoW, Ed25519 ephemeral session signing, WebSocket connection with exponential backoff reconnect, parallel session management, callback queue, and sandbox auto-negotiation via dynamically imported local GGUF models.
- **`ClinchSeller`** — server-side seller agent. Loads a permanent Ed25519 private key (no JWTs, no expiry), signs endpoint updates cryptographically, and exposes `signAsSeller` for co-signing verified buyer agreement artifacts.
- **`buildAgentPrompt()`** — generates a universally formatted negotiation system prompt from live session state, usable with any external LLM (Claude, Gemini, GPT-4o, Ollama/Llama3).
- **Blind Key Pass** *(Constructor configured)* — credential vault that silently injects API keys at the transport layer of handshakes, completely outside the AI context window.

### [`clinch-cli`](https://github.com/publicstringapps/clinch-cli) — The Client

The reference buyer agent and developer tool.

- **Constraint Parser** — parses plain natural language via a local Ollama instance running `llama3` into a strict, signed JSON constraint vector.
- **Background Daemon** — runs `clinch start` to hold active websocket channels open, stream real-time events, and execute callbacks or webhooks.
- **Local Vault** — uses AES-256-GCM encryption for storing your permanent identity keys, session states, and credentials locally in `~/.clinch/`.

---

## 🚀 Quickstart

```bash
npm install -g agent-clinch
clinch init

# Search for sellers
clinch discover "domain_name"

# Negotiate
clinch negotiate "domain clinchprotocol.io under 800 dollars" --target cloudflare-domains.anp

# View active and completed deals
clinch status
clinch deals
```

**Build a seller agent in under 10 minutes:**

```javascript
const { ClinchSeller } = require('clinch-core');

const seller = new ClinchSeller({ privateKeyHex: process.env.SELLER_PRIVATE_KEY });

await seller.registerNode(
  'mystore.anp',
  'https://api.mystore.com/anp/v1',
  ['electronics'],
  ['price_flex', 'bundle'],
  { supported_modes: ['ANP/C'] }
);

// Your Express routes handle /anp/v1/handshake and /anp/v1/sign_request
// seller.signAsSeller() validates the co-signatures of incoming deal artifacts
```

Your seller is now discoverable by every Clinch buyer agent on the network.

---

## 🛠 Open Protocol Milestones

The Clinch Protocol is an open, evolving standard. These are the active design areas open for community contribution:

- [ ] **Mobile Application:** Develop the official Clinch mobile app (iOS/Android) to bring local, on-device agent negotiations to consumer smartphones via `clinch-core`.
- [ ] **Multi-Agent Bidding Protocols:** Formalize sub-modes for multi-agent sealed-bid (`Bidding1`) and open conversational auctions (`Bidding2`).
- [ ] **ONNX / WASM Model Export** — compile the edge negotiation model for zero-dependency browser execution.
- [ ] **Escrow/Commit Mediation:** Implement a standardized cryptographic escrow contract for V2 when sellers go offline between `ACCEPT` and `COMMIT` states.
- [ ] **Multi-Item Basket Handshakes:** Design protocol schemas to handle multi-item baskets (`ANP-BASKET/0.1`) in a single negotiation session with partial acceptance semantics.
- [x] **Ginger Calendar Broker:** Reference Node.js implementation of a P2P calendar coordinating seller node.
- [ ] **Local-Only Reputation Incentives:** Design privacy-preserving proof systems to reward local-only buyer agents who securely report session metrics to the Registry.
- [ ] **Rideshare Driver Daemon:** Open-source a reference Node.js implementation for a dynamic ride-hailing seller node.

---

## 🤝 Contributing

Clinch is a community protocol. The spec is open. The codebase is open.

If you want to propose a protocol change, add a category schema, or build a compliant seller agent for your platform:

```text
1. Open an issue or pull request in the relevant repository.
2. For protocol changes: draft an ANP-RFC explaining the problem, 
   proposed payload format, and backward compatibility impact.
3. For governance or direct inquiries: publicstring.care@gmail.com
```

The network is only as strong as the agents on it.

---

<div align="center">

**Clinch Protocol** · ANP v0.2.1 Draft · MIT License

*Stop clicking. Start negotiating.*

</div>
