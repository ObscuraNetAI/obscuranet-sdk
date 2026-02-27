# ObscuraNet SDK

### Build Private Systems. Not Just Applications.

The ObscuraNet SDK is the official developer interface for building on ObscuraNet, a serverless AI-powered privacy infrastructure designed for encrypted execution, identity abstraction, and metadata resistance.

This SDK enables:

- Identity-free account creation
- Encrypted peer-to-peer messaging
- Private transaction execution
- Cryptographic proof generation and verification
- Local AI engine interaction
- Node communication and routing
- Swap and liquidity integration

ObscuraNet enforces privacy at the protocol layer, not the UI layer.

---

## Table of Contents

- [Installation](#installation)
- [Architecture Overview](#architecture-overview)
- [Core Modules](#core-modules)
- [Identity Model](#identity-model)
- [Network Layer API](#network-layer-api)
- [Local AI Engine API](#local-ai-engine-api)
- [Transaction Layer](#transaction-layer)
- [Cryptographic Toolkit](#cryptographic-toolkit)
- [Swap Engine](#swap-engine)
- [Environment Configuration](#environment-configuration)
- [Security Guidelines](#security-guidelines)
- [Performance Considerations](#performance-considerations)
- [Examples](#examples)
- [Contributing](#contributing)
- [License](#license)

---

## Installation

```bash
npm install obscuranet-sdk
```

Or build from source:

```bash
git clone https://github.com/ObscuraNetAI/obscuranet-sdk.git
cd obscuranet-sdk
npm install
npm run build
```

---

## Architecture Overview

The SDK mirrors the ObscuraNet protocol layers:

```text
Application Layer
        ↓
ObscuraClient
        ↓
Account | Network | AI | Proof | Transaction | Swap
        ↓
Encrypted Node Communication
```

Each module operates independently and is composable.

---

## Core Modules

### 1. Account Module

Identity abstraction without exposure.

Capabilities:

- Generate pseudonymous keypairs
- Rotate identity keys
- Sign messages securely
- Validate signatures

```ts
const account = await client.account.create();
```

### 2. Network Module

Encrypted transport and node routing.

Capabilities:

- Node discovery
- Secure P2P routing
- Encrypted packet transmission
- Network status queries

```ts
await client.network.getStatus();
```

### 3. Local AI Engine

Run AI inference locally without cloud exposure.

Capabilities:

- Execute local AI prompts
- Encrypted AI query transport
- AI model selection
- Resource monitoring

```ts
const result = await client.ai.execute({
  model: "local-obscura-1",
  input: "Analyze encrypted input"
});
```

### 4. Transaction Module

Private transaction execution.

Capabilities:

- Sign transactions
- Broadcast securely
- Monitor confirmations
- Estimate fees

```ts
await client.transaction.send({
  from: account,
  to: recipient,
  amount: 5
});
```

### 5. Cryptographic Proof Engine

Built-in cryptographic verification tools.

Supported:

- Merkle proof generation
- zk-SNARK verification
- Signature validation
- Hash commitments

```ts
const proof = await client.proof.generateMerkleProof(data);
```

### 6. Swap Engine

Private asset routing and liquidity interaction.

Capabilities:

- Find optimal swap routes
- Execute swaps
- Check liquidity pools

```ts
const route = await client.swap.findRoute(tokenA, tokenB);
await client.swap.execute(route);
```

---

## Identity Model

ObscuraNet does not use:

- Email-based accounts
- KYC identity binding
- Central identity servers

Instead, it uses:

- Deterministic keypairs
- Pseudonymous addresses
- Rotatable identity layers

Identity exists cryptographically, not socially.

---

## Environment Configuration

Supported environments:

| Environment | Purpose |
| ----------- | ------- |
| local | Development |
| testnet | Testing |
| mainnet | Production |

Example:

```ts
const client = new ObscuraClient({
  network: "testnet",
  endpoint: "https://testnet.node.obscuranet.ai"
});
```

---

## Security Guidelines

- Never store private keys in plaintext
- Use hardware-backed signing when possible
- Verify all cryptographic proofs
- Never expose local AI execution data externally
- Run SDK in isolated execution environments for production

ObscuraNet assumes adversarial conditions.

---

## Performance Considerations

- Local AI execution depends on device hardware
- Proof generation may be resource intensive
- Swap routing uses network query optimization
- Node latency varies based on geographic routing

---

## Examples

Run basic example:

```bash
npx ts-node examples/basic-usage.ts
```

Build and run compiled:

```bash
npm run build
node examples/dist/basic-usage.js
```

Available example:

- `basic-usage.ts`

---

## Contributing

We welcome serious contributors.

Before submitting:

- Follow TypeScript strict mode
- Include test coverage
- Document new API surfaces
- Ensure cryptographic correctness

Open a Pull Request with detailed explanation.

---

## License

MIT License

---

## About ObscuraNet

ObscuraNet is a decentralized AI-powered privacy network built for encrypted communication, identity-free access, and autonomous digital infrastructure.

No servers.
No metadata trails.
No external control.

Privacy is enforced at the protocol layer.
