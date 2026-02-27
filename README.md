# Obscuranet SDK

The official SDK for integrating Obscuranet's zero-trace, RAM-only, self-erasing execution model into your own applications.

---

## Overview

The **Obscuranet SDK** gives developers access to the same core systems that power Obscuranet: volatile execution, memory sharding, multi-hop relay routing, and total post-operation erasure. Everything the SDK does happens inside RAM, and everything is destroyed as soon as you trigger TraceKill or the session expires.

---

## Key Features

- **Ephemeral Sessions** – All operations occur inside temporary, RAM-only execution cells.
- **Lattice-Sharded Keys** – Your private keys are never whole; they exist only as encrypted shards inside memory.
- **In-RAM Signing** – Transactions are signed without ever exposing the seed or private key.
- **NullPipe Routing** – Built-in seven-hop self-destructing relay system that removes metadata and timing fingerprints.
- **TraceKill™** – Programmatic destruction of memory, shards, relays, and all runtime processes.
- **$SERRA Token Hooks** – Pay for sessions, upgrade relays, activate stealth modes, and integrate token utilities.

---

## Installation

### JavaScript / TypeScript
```bash
npm install serra-sdk
```

### Python
```bash
pip install serra-sdk
```

---

## Quick Start

### 1. Initialize the SDK
```js
import { SerraSDK } from "serra-sdk";

const sdk = SerraSDK.init({
  network: "mainnet-null",
  tokenProvider: (amount) => paySERRA(amount),
});
```

### 2. Create an Ephemeral Session
```js
const session = await sdk.session.create({
  cost: 0.001,
  relays: 7,
  shardCount: 5,
  traceKillMs: 100,
});
```

### 3. Generate and Shard a Seed
```js
const seed = await session.crypto.generateSeed({ entropyBits: 256 });
const shards = await session.crypto.latticeShard(seed, { parts: 5, threshold: 3 });
```

### 4. Sign a Transaction in RAM
```js
const signed = await session.crypto.signWithShards({
  shardHandles: shards.map(s => s.id),
  payload: {
    to: "0xdeadbeef...",
    value: "0.5 ETH",
  },
  threshold: 3,
});
```

### 5. Relay Through NullPipe
```js
const receipt = await session.network.sendViaNullPipe({
  signedTx: signed,
  hops: 7,
});
```

### 6. Trigger TraceKill
```js
await session.tracekill.trigger();
```

---

## CLI Usage

```bash
serra session create --cost 0.001 --relays 7 --shards 5
serra key gen --entropy 256
serra sign --shards <id1,id2,id3> --file unsigned.json
serra send --via nullpipe --hops 7 signed.json
serra tracekill --session <id>
```

---

## SDK Capabilities

- Build bots, wallets, and tools that leave **zero logs**.
- Execute sensitive actions inside **volatile environments**.
- Construct workflows that **self-destruct on exit**.
- Enable **secure signing** without ever touching disk.
- Add **advanced relay routing** to any application.
- Use $SERRA to power private computation.

---

## Security Model

- **No Disk Writes** – Everything stays in RAM.
- **Sharded Seeds** – Private keys can never be reconstructed outside the session.
- **Relay Obfuscation** – Network metadata is destroyed.
- **Memory Wipe** – Guaranteed clean state after TraceKill.

This SDK cannot prevent misuse. You are responsible for ensuring compliance with laws and platform policies when integrating Obscuranet technologies.

---

## Repository

GitHub: https://github.com/obscuranet/SDK

---

## License

Open-source license included in the repository.

---

## Contributions

Pull requests, issues, and improvements are welcome. If you're building privacy tech, this SDK is for you.

---

## Code Samples (Added)

Below are ready-to-run example files demonstrating the core SDK flows. Drop these into your docs or examples directory.

---

### JavaScript Example — `examples/js/ephemeral_session.js`
```js
// examples/js/ephemeral_session.js
import { SerraSDK } from 'serra-sdk';

async function main() {
  // Initialize SDK (implement paySERRA to satisfy tokenProvider)
  const sdk = SerraSDK.init({
    network: 'mainnet-null',
    tokenProvider: async (amount) => await paySERRA(amount),
  });

  // Create ephemeral session
  const session = await sdk.session.create({
    cost: 0.001,
    relays: 7,
    shardCount: 5,
    traceKillMs: 100,
  });
  console.log('Session ready', session.id);

  // Generate seed and shard in-RAM
  const seed = await session.crypto.generateSeed({ entropyBits: 256 });
  const shards = await session.crypto.latticeShard(seed, { parts: 5, threshold: 3 });
  console.log('Shards created', shards.map(s => s.id));

  // Build minimal transaction payload
  const tx = {
    to: '0xdeadbeefdeadbeefdeadbeefdeadbeefdeadbeef',
    value: '0.01 ETH',
    data: '0x',
  };

  // Sign with shards inside session
  const signed = await session.crypto.signWithShards({
    shardHandles: shards.map(s => s.id),
    payload: tx,
    threshold: 3,
  });
  console.log('Signed in-RAM', signed.id);

  // Send via NullPipe
  const receipt = await session.network.sendViaNullPipe({ signedTx: signed, hops: 7 });
  console.log('Transaction relayed', receipt.txHash);

  // Trigger TraceKill and exit
  await session.tracekill.trigger();
  console.log('TraceKill complete');
}

main().catch(err => { console.error(err); process.exit(1); });
```

---

### Python Example — `examples/py/ephemeral_session.py`
```py
# examples/py/ephemeral_session.py
from serra_sdk import SerraSDK

def pay_serra(amount):
    # Implement your off-chain payment flow here
    return True

sdk = SerraSDK.init(network='mainnet-null', token_provider=pay_serra)

session = sdk.session.create(cost=0.001, relays=7, shard_count=5, trace_kill_ms=100)
print('Session', session.id)

seed = session.crypto.generate_seed(entropy_bits=256)
shards = session.crypto.lattice_shard(seed, parts=5, threshold=3)
print('Shards', [s.id for s in shards])

payload = { 'to': '0xdeadbeefdeadbeefdeadbeefdeadbeefdeadbeef', 'value': '0.01' }
signed = session.crypto.sign_with_shards(shard_handles=[s.id for s in shards], payload=payload)
receipt = session.network.send_via_nullpipe(signed_tx=signed, hops=7)
print('Relayed', receipt.tx_hash)

session.tracekill.trigger()
print('TraceKill triggered')
```

---

### CLI Example — `examples/cli/demo.sh`
```bash
#!/usr/bin/env bash
# examples/cli/demo.sh
set -e

# Create session
serra session create --cost 0.001 --relays 7 --shards 5

# Generate seed
serra key gen --entropy 256

# Sign file (unsigned.json produced by your flow)
serra sign --shards <id1,id2,id3> --file unsigned.json

# Send via nullpipe
serra send --via nullpipe --hops 7 signed.json

# Wipe session
serra tracekill --session <id>
```

---

## Example Outputs

**JS console**
```
Session ready 9b1f7f4a-... 
Shards created ["shard-1","shard-2","shard-3","shard-4","shard-5"]
Signed in-RAM sgn-0x...
Transaction relayed 0xabc123...
TraceKill complete
```

**Python stdout**
```
session 9b1f7f4a-...
Shards ["shard-1","shard-2","shard-3","shard-4","shard-5"]
Relayed 0xabc123...
TraceKill triggered
```

---

